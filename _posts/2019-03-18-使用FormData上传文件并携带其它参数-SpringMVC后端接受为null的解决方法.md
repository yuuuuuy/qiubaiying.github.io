---
layout:     post
title:      使用FormData上传文件和其它参数，SpringMVC后端获取参数为null的解决方法
subtitle:   FormData上传文件+其它参数，Spring后端获取不到
date:       2019-03-18
author:     Jae
header-img: img/springmvc.png
catalog: true
tags:
    - Java
    - SpringMVC
---

#### 1、问题产生

在写一个基于SpringMVC的前后端分离的项目中，前端通过Ajax的POST方法将文件上传到后端，前端上传的除了文件
还有其它字段，于是使用FormData对象来存储并post到后端；
后端的controller中通过request.getParameter("key")来获取前端提交的数据，但是遇到问题就是获取的值一直为NULL。

前端请求的代码：

    var formData = new FormData();
    var shopImg = $('#shop-img')[0].files[0]; // 这里是文件
    formData.append('shopImg', shopImg);
    formData.append('username', 'jae');
    formData.append('password', '123456');
    $.ajax({
			url : url,
			type : 'POST',
			data : formData,
			enctype: 'multipart/form-data',
			processData: false,  // 不对数据进行处理
            contentType: false,
            cache: false,
			success : function(data) {
				if (data.success) {
					$.toast('提交成功！');
				}
				else{
					$.toast('提交失败！' + data.errMsg);					
				}
			}
		});

后端获取参数方式：

    request.getParameter("username");
    null

#### 2、调试思路

##### 2.1 什么是FormData

FormData对象用来将数据编译成键值对，以便使用XMLHttpRequest来发送数据，其主要用于发送表单数据，但是也可以用来发送带键的数据，而
独立于表单的使用。

下面是前端发送的请求信息

    Provisional headers are shown
    Accept: */*
    Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryBevlA3nAb5XKi0XB
    Origin: http://127.0.0.1:8080
    Referer: http://127.0.0.1:8080/shopadmin/shopoperation
    User-Agent: Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.62 Mobile Safari/537.36
    X-Requested-With: XMLHttpRequest
    \_: 1552878374830
    ------WebKitFormBoundaryBevlA3nAb5XKi0XB
    Content-Disposition: form-data; name="shopImg"

    undefined
    ------WebKitFormBoundaryBevlA3nAb5XKi0XB
    Content-Disposition: form-data; name="shopStr"

    {"shopName":"12","shopAddr":"12","phone":"12","shopDesc":"","shopCategory":{"shopCategoryId":2},"area":{"areaId":2}}
    ------WebKitFormBoundaryBevlA3nAb5XKi0XB--

##### 2.2 SpringMVC中如何处理前端的请求

    1）用户发送请求到前端控制器DispatcherServlet
    2）DispatcherServlet收到请求调用处理器映射器HandlerMapping
    3）HandlerMapping根据请求的URL找到具体的处理器，生成处理器执行链HandlerExecutionChain返回给DispatcherServlet
    4）DispatcherServlet根据处理器Handler获取处理器适配器Handler Adapter执行HandlerAdapter处理一系列操作，如参数封装，数据格式转换
    数据验证等
    5）执行处理器Handler（我们自己写的Controller）
    下面略

##### 2.3 DEBUG跟踪请求

<1> 当前端请求到达SpringMVC的DispatcherServlet时，首先会到达它的doService()方法。代码如下

    @Override
    protected void doService(HttpServletRequest request, HttpServletResponse response) throws Exception {
        logRequest(request);

        // Keep a snapshot of the request attributes in case of an include,
        // to be able to restore the original attributes after the include.
        Map<String, Object> attributesSnapshot = null;
        if (WebUtils.isIncludeRequest(request)) {
            attributesSnapshot = new HashMap<>();
            Enumeration<?> attrNames = request.getAttributeNames();
            while (attrNames.hasMoreElements()) {
                String attrName = (String) attrNames.nextElement();
                if (this.cleanupAfterInclude || attrName.startsWith(DEFAULT_STRATEGIES_PREFIX)) {
                    attributesSnapshot.put(attrName, request.getAttribute(attrName));
                }
            }
        }

        // Make framework objects available to handlers and view objects.
        request.setAttribute(WEB_APPLICATION_CONTEXT_ATTRIBUTE, getWebApplicationContext());
        request.setAttribute(LOCALE_RESOLVER_ATTRIBUTE, this.localeResolver);
        request.setAttribute(THEME_RESOLVER_ATTRIBUTE, this.themeResolver);
        request.setAttribute(THEME_SOURCE_ATTRIBUTE, getThemeSource());

        if (this.flashMapManager != null) {
            FlashMap inputFlashMap = this.flashMapManager.retrieveAndUpdate(request, response);
            if (inputFlashMap != null) {
                request.setAttribute(INPUT_FLASH_MAP_ATTRIBUTE, Collections.unmodifiableMap(inputFlashMap));
            }
            request.setAttribute(OUTPUT_FLASH_MAP_ATTRIBUTE, new FlashMap());
            request.setAttribute(FLASH_MAP_MANAGER_ATTRIBUTE, this.flashMapManager);
        }

        try {
            doDispatch(request, response);  // 接下来调用该函数--<2>
        }
        finally {
            if (!WebAsyncUtils.getAsyncManager(request).isConcurrentHandlingStarted()) {
                // Restore the original attribute snapshot, in case of an include.
                if (attributesSnapshot != null) {
                    restoreAttributesAfterInclude(request, attributesSnapshot);
                }
            }
        }
    }

<2> 接着会调用doDispatch(request, response)

    protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
        HttpServletRequest processedRequest = request;  // 已经处理的请求
        HandlerExecutionChain mappedHandler = null;     // 处理器映射器
        boolean multipartRequestParsed = false;         // 是否已经解析了multipartRquest

        WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

        try {
            ModelAndView mv = null;  // 模型视图
            Exception dispatchException = null;

            try {
                processedRequest = checkMultipart(request);  // 首先检查该请求是否是MultipartRequest 我们再跟进去。----<3>

                // 这里对比两次request是否为相同，如果相同说明就不是multipartRequest
                // 于是将request作为HttpServletRequest对象处理，这时候在我们的controller中
                // 通过requet.getParademeter("key")是获取到的key值为null
                multipartRequestParsed = (processedRequest != request);

                // Determine handler for the current request.
                mappedHandler = getHandler(processedRequest);
                if (mappedHandler == null) {
                    noHandlerFound(processedRequest, response);
                    return;
                }

                // Determine handler adapter for the current request.
                HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

                // Process last-modified header, if supported by the handler.
                String method = request.getMethod();
                boolean isGet = "GET".equals(method);
                if (isGet || "HEAD".equals(method)) {
                    long lastModified = ha.getLastModified(request, mappedHandler.getHandler());
                    if (new ServletWebRequest(request, response).checkNotModified(lastModified) && isGet) {
                        return;
                    }
                }

                if (!mappedHandler.applyPreHandle(processedRequest, response)) {
                    return;
                }

                // Actually invoke the handler.
                mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

                if (asyncManager.isConcurrentHandlingStarted()) {
                    return;
                }

                applyDefaultViewName(processedRequest, mv);
                mappedHandler.applyPostHandle(processedRequest, response, mv);
            }
            catch (Exception ex) {
                dispatchException = ex;
            }
            catch (Throwable err) {
                // As of 4.3, we're processing Errors thrown from handler methods as well,
                // making them available for @ExceptionHandler methods and other scenarios.
                dispatchException = new NestedServletException("Handler dispatch failed", err);
            }
            processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
        }
        catch (Exception ex) {
            triggerAfterCompletion(processedRequest, response, mappedHandler, ex);
        }
        catch (Throwable err) {
            triggerAfterCompletion(processedRequest, response, mappedHandler,
                    new NestedServletException("Handler processing failed", err));
        }
        finally {
            if (asyncManager.isConcurrentHandlingStarted()) {
                // Instead of postHandle and afterCompletion
                if (mappedHandler != null) {
                    mappedHandler.applyAfterConcurrentHandlingStarted(processedRequest, response);
                }
            }
            else {
                // Clean up any resources used by a multipart request.
                if (multipartRequestParsed) {
                    cleanupMultipart(processedRequest);
                }
            }
        }
    }

<3> 跟进到checkMultipart(request)中

    protected HttpServletRequest checkMultipart(HttpServletRequest request) throws MultipartException {
        // 这里首先判断this.multipartResolver是否为null我们在该文件开始部分的注释中找到这样的说明
        <li>The dispatcher's strategy for resolving multipart requests is determined by a
              {@link org.springframework.web.multipart.MultipartResolver} implementation.
              Implementations for Apache Commons FileUpload and Servlet 3 are included; the typical
              choice is {@linkorg.springframework.web.multipart.commons.CommonsMultipartResolver}.
             The MultipartResolver bean name is "multipartResolver"; default is none.
        // 从文档中可以看到，对于multipart requests默认是没有提供解析器的，所以下面的if判断就为false
        最后返回的request和入参的request一样，再回到上面的代码

        if (this.multipartResolver != null && this.multipartResolver.isMultipart(request)) {
            if (WebUtils.getNativeRequest(request, MultipartHttpServletRequest.class) != null) {
                if (request.getDispatcherType().equals(DispatcherType.REQUEST)) {
                    logger.trace("Request already resolved to MultipartHttpServletRequest, e.g. by MultipartFilter");
                }
            }
            else if (hasMultipartException(request) ) {
                logger.debug("Multipart resolution previously failed for current request - " +
                        "skipping re-resolution for undisturbed error rendering");
            }
            else {
                try {
                    return this.multipartResolver.resolveMultipart(request);
                }
                catch (MultipartException ex) {
                    if (request.getAttribute(WebUtils.ERROR_EXCEPTION_ATTRIBUTE) != null) {
                        logger.debug("Multipart resolution failed for error dispatch", ex);
                        // Keep processing error dispatch with regular request handle below
                    }
                    else {
                        throw ex;
                    }
                }
            }
        }
        // If not returned before: return original request.
        return request;  返回到----<2>
    }

#### 3、产生原因

产生的原因就是我们没有提供org.springframework.web.multipart.MultipartResolver，导致我们的multipart/formdata请求无法被正确的解析，进而导致我们在controller中无法获取到参数。

#### 4、解决方法

在Spring-mvc.xml文件中配置相应的bean，这样在checkMultipart(request)就可以将HttpServletRequest进行包装

    <bean id="multipartResolver"
        class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="maxUploadSize" value="10485760"/>
        <property name="maxInMemorySize" value="40960" />
    </bean>

#### 5、收获

再遇到前端请求后端无法获取预期的参数时，最好的调试方法就是根据SpringMVC请求的过程从源头跟进，这样能大大节省解决问题的时间。
