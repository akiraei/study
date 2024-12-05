```mermaid
%%{init: {'theme': 'forest'}}%%
mindmap
  root((DI))
	  Ioc Container
		  implements ApplecationContext
		  implements BeanFactory
	  ApplicationContext
		  Interface
		  Methods
			  getBean(String name)
			  getBean(Class<T> requiredType)
			  publishEvent(Object event)
			  getEnvironment()
		  
	  BeanFactory
		  Interface
	  
```

