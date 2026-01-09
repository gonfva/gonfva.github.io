---title: Notes to a newbie spring-mvc developer
date: 2012-01-13T11:45:46Z
categories: [Developer got ya]
tags: [Developer]
---


1. Activate log4j. Without it you're lost

2. Look through the log to see if you're getting the RequestMapping through Spring.

3. If you are not despite correct annotations, see [this post](http://www.vaannila.com/spring/spring-annotation-controller-1.html). In particular if you've got Spring Security installed, for Goodness sake, don't forget these lines:
```
<beans:bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping">
  <beans:bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
  </beans:bean>
</beans:bean>
```

4. Just in case you decided to change the mappings, and you continue to see the old ones in the logs, clean and rebuild.
