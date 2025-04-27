# Spring-Cloud-Config



Spring Cloud Config Server :
--------------------------


			Example:    Centralized Configuration
			            -------------------------
						
			
						Limits MicroService 				MicroService X						MicroService Y
						
						
														Spring Cloud Config Server
														
															
															  Git Repo
															  

			Step 0 :

						In the limit service mention, configserver port and details in the application.properties.
						
						spring.application.name=limits-service    // --------> has the id talk to the config server.
						spring.config.import=optional.configserver:http://localhost:8888
						
						spring.profiles.active=dev
						spring.cloud.config.profile=dev
						
						limits-service.minimum=3
						limits-service.maximum=997
						
			Step 1 :

						Add dependency in pom.xml
						
						<dependency>
							<groupId>org.springframework.cloud</groupId>
							<artifactId>spring-cloud-starter-config</artifactId>
						</dependency>
						
			Step 2:

					   git-localconfig.repo....
					   
					   limits-service.properties
					   -------------------------
					   
					   limits-service.minimum=4
					   limits-service-maximum=996
					   

						
			Step 3 :

						Configure with Git and update in the application.properties file.
						
						spring.application.name=spring-cloud-config-server
						server.port=8888
						spring.cloud.config.server.git.uri=file:///in........ git-repo-localpath.git
						
			Step 4 :

						Add the following annotation in SpringBootApplication.
						
						@EnableConfigServer 
						
			Step 5 :

						Spring Cloud Config Server port :8888
						
						Browser : localhost:8888/limits-service/default
						

			Step 6 :

						Setting Profiles for dev and qa for the limits-service.properties.
						
						git repo create these two files.
						
						1. limits-service-dev.properties
						2. limits-service.qa.properties
						
			Step 7 :

						Need to pick dev configuration file.
						
						localhost:8888/limits-service/dev



			How to set Dynamic port in MicroService?


			Step 1:

					
					In controller class.
					
					import org.springframework.core.env.Environment;
					
					@Autowired
					private Environment environment 
					
					String port = environment.getProperty("local.server.port");
					currencyExchange.setEnvironment(port);     //  ---> currencyExchange is the service.
					
			
Feign Client :
-------------


			Step 1 : 

						Added dependency in pom.xml ( feign starter projects )
					
			Step 2 :

						@EnableFiegnClients // <- add this annotation ins @SpringBootApplication class. [ in the application clas ]
					
			Step 3 :

						@FiegnClient(name="currency-exchange",url="localhost:8000")
						

Naming Server :
-------------
						
						
		CurrencyConversion MicroService        Currency Exchange MicroService			MicroService X
		
				                                           |
														   
												Naming Server or Service Registry
												
												
			

Naming Server : { Discovery Server - Eureka Server ]
----------------------------------------------------

			Step 1 :
			
			            Eureka Server [ SPRING CLOUD DISCOVER - pom.xml ] add dependency.
						Dev tools
						Actuator
			
			Step 2 :
			
						In the application class, enable the following annotation.
						
						@EnableEurekaServer 
						@SpringBootApplication
						
						
			Step 3 :
			
						application.properties
						---------------------
						
						spring.application.name=naming-server
						server.port=8761
						eureka.client.register-with-eureka=false   // <- dont register with specific server itself.
						eureka.client.fetch-registry=false
			
			Step 4 : 
			
						eureka server is started.
						
						In browser : localhost:8761/
						
Eureka Client :
--------------

		   Step 1 :
		   
		               pom.xml 
					   
					   Already these dependencies are added.
					   
					   starter-actuator
					   starter-web
					   starter-config
					   
					   Add New :
					   --------
					   
					   <dependency>
					      <groupId>org.springframework.cloud</groupId>
						  <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
					   </dependency>
					   
					   
			Step 2 :
					
					   application.properties
						---------------------
  					 
					   eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka
					   
Spring Cloud :
------------
			Key Solutions :
			--------------
			
			1. Centralized Configuration 
			
					* Manage configuration for multiple microservices in central GIT repository
			
			2. Load Balancing 
			
					* Distributes requests across active instances of microservices dynamically.
			
			3. Service Discovery
			
					* enable automatic discovery of microservices
					
			4. Distributed Tracing
			
					* Trace requests across microservices
					
			5. Edge Server
				
					* Single Entry Point: implement common features like authentation.
			
			6. Fault Tolerance 
			
					* Ensure that failure in one microservice does not cascade and make other microservices to failure
