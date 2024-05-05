[toc]

# SpringMVC工作流程

<img src="img/image-20240505113231552.png" alt="image-20240505113231552" style="zoom: 50%;" />

用户请求➡️前端控制器 DispatcherServlet 接受并拦截请求

➡️Dispatcherservlet 调用 HandlerMapping 处理映射器，然后 HandlerMapping 根据请求 url 查找具体的 Handler 然后返回处理器执行链给 Dispatcherservlet

➡️Dispatcherservlet 调用 HandlerAdapter 处理器适配器调用刚刚拿到的执行链中的 Handler，然后返回视图逻辑名或模型 ModelAndView 给 HandlerAdapter，然后再返回给 Dispatcherservlet，如果方法上加了 @ResponseBody，那么就把返回值直接返回给浏览器

➡️﻿﻿﻿如果方法上没有加 @ResponseBody，DispatcherServlet 调用视图解析器 ViewResolver 来解析 HandlerAdapter 传递的 ModelAndView，解析完再返回给 DispatcherServlet

➡️DispatcherServlet 调用具体的视图比如 Jsp，freemaker 进行渲染，然后返回给客户端