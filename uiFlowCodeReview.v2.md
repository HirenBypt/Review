# Final Code Review

1. Very not understandable commit names and descriptions. I got email with commits history like this: 
```
Merge branch 'code-refactor' of https://github.com/ashwaksk017/uiFlowDesign into code-refactor
Changes in response and add service and repo layer in flowController
conflict changes commit
merge
Add Comment documetation
Add service and repository layer in flowGroupController
Component And Project Controller changes
Resolved Conflict and Merge
Small Change
some changes to make dto and use that
URL Changes
merge
Merge
initial changes
URL Change Flow-gruop
Small Change
Removing unnecessary files
merge
removed ambigiuos call
Small Changes
initial changes
Merge
Resolving Conflict in following files..
project related changes
merge
added loger
Small Change
Component Controller API changes, url Changes ,
``` 
It's great to use convenient for commits namings ([example](https://www.conventionalcommits.org/en/v1.0.0-beta.3/))

*(Comment Type: Suggestion) Sure, we can follow the same. In most of the cases we add the small description which seems understandble based on the small description. Though we can add prefix as suggested.*

1. Configure application very difficult and not intuitive. We need to create configuration file at `root` directory (it's very strange) 
And it's very helpful to have example of configuration filled with placeholders,  for example
```
spring.data.mongodb.database=placeholder
spring.data.mongodb.host=placeholder
spring.data.mongodb.port=placeholder
``` 
Better way is using Spring profiles, so there will be general config with placeholders, for each environment (dev, prod) You 
will extends this profile and overwrite only needed fields. It's bad to force create configuration file in root and put there strange strings.
Developer can add new functionality and forgive to add description in Readme, so application will crash.

*To keep separted config file is to keep the system level config aside (ex. db config). The reason for it is to not change the application.properties file for running the project. As we have separated config file for each env, we dont need spring profiles. And for the crash thing, if system level new config parameter is added, even with spring profile implemented if developer doesn't ovrride, its gonna crash even. So developer have to override those either in the separated config file or dev config file to run it I think :). For the path of config file, developer can change in ReactApplication.java as per the ReadMe file. root was just for easy way.*

1. `set system env variables based on the EnvironmentConstants.java file.` Person who wants to run application need to 
find source file (`EnvironmentConstants`), find how set them in his OS and be nervous about correctness of his actions. 
It's better to use environments constants from Spring properties. So 'user' will see all required configurations in one file.
It will be great to have Bash (for Unix) and Bat (for Windows) script to setup environment variable with specified params.

*using Environment variables just for storing secure details. Sure we can add script files for setting up but I think setting up env varibales should not make developer nervous.* 

1. Add maven profiles only for dependency for `Spring-boot-dev-tools` is overkill. [Documentation](`https://docs.spring.io/spring-boot/docs/current/reference/html/using-boot-devtools.html`)
specified how add this dependency to development use only.  

*Sure we can use that, Agree. Reason to add maven profile is I was planning to add some properties there like skip tests, plugins etc. Though for now I can remove that if its needed.*

1. It's better to specify dependency's version clearly. Dependency's developers can change logic or API, so this will lead to bugs.

*I think I have already added the versions in properties(clear way) when there is no overriding from Spring boot*

1. Developers add dependency for Swagger (API description), but there is no information in readme how to use it. Swagger need to 
easy communication between BE and FE developers to specify API.

*Yes I will add that in readme.*

1. (Minor) Add empty lines in `pom.xml` between sections for better readability and add comments for dependencies what for this dependency is.

*ok*

1. File `ATPMTConstants.java` contains constant `BASE_PATH` which equals to `"img/web_elements"`. 
I think it's better to name it like `IMAGE_BASE_PATH`. 
P.S. There is a little life hack for classes with constants, not to write `final puvblic static` You can rewrite that as interface, where 
all fields by default public, static and final. 

*ok*

1. File `ElementType.java` contains name `FLOWGROUP`, by conversation when name uses UPPER_CASE, then use underscore to separate words.

*ok*

1. I think file `EnvironmentConstants` can be deprecated and deleted. 



1. File `Error.java` can be more shorter with Lombok: `@AllArgsConstructor(access = AccessLevel.PRIVATE)` to generate private constructor, `@Getter` - for getters. 

*yeah. ok.*

1. Params in controllers often written in one line, so it makes controller readability worse. 

*will format that*

1. In many places used approach to make relations between two entities (for example Project and Group). We want to 
make relation between Group and Project. We send to BE Group and ProjectId. BE creates ingot Project with only Id and 
relates it to group. But there is no check is project with such Id exists in DB.

*BE does not create project transitionally but just mapping it. And practically if the projectId won't have in DB, FE won't have even.*

1. Mess with Models and DTO. Main use of DTO, when we have information from different sources and we concatenate is together. 
For example if need `Component` with `Project` information then there we use DTO, where we specify required fields from 
`Component` and `Project`. In many places Models only copied to class named ModelDTO and returns to FE or make excess request 
 to DB only to return DTO. If endpoint return model as it stored in DB, then return Model. DTO useful with some extra fields or data aggregation.  
 
*DTO is used to not to send or expose unnecessary fields to FE. Like if FE wants Flow Name and ID only, if we send Model itself to FE, they will get array of Components, FlowGroup etc.*

1. `FlouController` contains logic to make relations between models, which moved to `Services` in other `Controllers`.

*Will update that*

1. In few places developer get ArrayList from model, add value to this list and for some reasons set this list to model back. 
But it superfluous. When we get ArrayList from model, we get link to this list and in the end we set the same link to model. 

*can you please let me know the place any? So I can follow your comment well.*

1. Many `Repository` uses `public` word, but all interfaces methods are public by default. 

*yes. will remove that.*

1. There are custom repositories, but they are unused, so we can delete them.

*can you let us know the respository names?*

1. Endpoints still make confused about data model's relations. 

*can you let us know the places with suggested endpoint url?*

1. Many code duplicates in controllers

*can you let us know the places?*

