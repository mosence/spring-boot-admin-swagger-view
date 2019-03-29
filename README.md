# spring-boot-admin-swagger-view
lockup endpoint id 'api-docs' in such springboot, then use Swagger-UI create an swagger-webside client.


for example:

using springfox's @EnableSwagger2 and create a bean Docket() to create /v2/api-docs path.

then using @Endpoint(id = "api-docs") to suport an endpoint like this.

~~~

@Component
@Endpoint(id = "api-docs")
public class ServletSwaggerApiEndpoint{

    @Resource
    private HttpServletRequest servletRequest;

    private DocumentationCache documentationCache;
    private ServiceModelToSwagger2Mapper mapper;

    public ServletSwaggerApiEndpoint(DocumentationCache documentationCache,
                                      ServiceModelToSwagger2Mapper mapper) {
        this.documentationCache = documentationCache;
        this.mapper = mapper;
    }
	
    @ReadOperation(produces = "application/json")
    public Swagger loadApiDocs() {
        String groupName = Docket.DEFAULT_GROUP_NAME;
        Documentation documentation = documentationCache.documentationByGroup(groupName);
        Swagger swagger = mapper.mapDocumentation(documentation);

        UriComponents uriComponents = componentsFrom(servletRequest, swagger.getBasePath());
        swagger.basePath(Strings.isNullOrEmpty(uriComponents.getPath()) ? "/" : uriComponents.getPath());
        swagger.setHost(uriComponents.getHost()+ ":" +uriComponents.getPort());
        
        return swagger;
    }
}

~~~
