# Prototype architecture

## Services

PrototypeBundle has been built on the microservice architecture pattern. 

Simple workflow of services could be included in one sentence :
**Contoller calls a service which is based on the routing.**
  1.  Service definition, location:PrototypeBundle\Resources\Config\services.yml:
```
  prototype.formtype:
        class: Core\PrototypeBundle\Form\FormType
        arguments: []        
        tags:
            -  { name: prototype.formtype } 

```


    a)    prototype.formtype - it is a name of service. It will be using by other classes for recognize a correct service. 
    b)    class path - full name of the service file
    c)    arguments:[] -  an array of service arguments, could be null, container 
    ```
    [@service_container] 
    
    ``` etc.
    d)    tags: - element of correct service identify, could be a few, e.g.:
    *    route - routing, only begining part is enough 
    *    entity - full name of entity
    *    actionId - there are two kinds, for routings and objects
    *   **Attention:** if name and route exist any other params are not neccesery
  2. Commands
Prototype bundle were prepared with a special commands for easiest work process. The solution is based on generators. The solution is hidden in automatically generate a new files. The specific commands allows to build almost every type of important file, e.g FormType etc.
Command looks like below:
```
prototype:generate 
```
param syntax:
```
Parameters: --withFormTypes; --withConfigServices
```



##Twigs

There are two types of twigs:
- element
- container;

**Element** - the content of page, the center part of it;
**Container** - the kind of wrapper of the content, the frame part of page.



##Configuration
There are two types of configuration files:
  1. Basic configuration location: Prototype Bundle\DependencyInjection\Configuration. This is built on a tree structure with TreeBuilder object. Default file always appears after generated new bundle. Includes information about basic elements of application, e.g. twigs. It could be overwritten by Controller(application), Service.


  2. Services configuration location: PrototypeBundle\Services\Config. The basis of this structure is merging of associative arrays process. Main pattern for create this kind of configuration looks like example below:

```
actions:
    create:
          templates:
              container:'Default\Container\create.html.twig'
              element:'Default\Element\create.html.twig'
    read:
          templates:
              container:'Default\Container\read.html.twig'
              element:'Default\Element\read.html.twig'
    update:
          templates:
              container:'Default\Container\update.html.twig'
              element:'Default\Element\update.html.twig'
    view:
          templates:
              container:'Default\Container\view.html.twig'
              element:'Default\Element\view.html.twig'
    list:
          templates:
              container:'Default\Container\list.html.twig'
              element:'Default\Element\list.html.twig'
    grid:
          templates:
              container:'Default\Container\grid.html.twig'
              element:'Default\Element\grid.html.twig'
    error:
          templates:
              container:'Default\Container\error.html.twig'
              element:'Default\Element\error.html.twig'
              
    routings:
          read:
                route: '*read'
                route_params: 'null'
          list:
                route: '*list'
                route_params: 'null'
          new:
                route: '*new'
```

First part of the structure includes name of controller action e.g. read, list, new etc. Templates are the main twigs with typical content depends on view type, element or container. Structure of routings has been built on the same structure as actions, with one additional field: route_params - parameters of routing.


##Controller

Main controller of PrototypeBundle is a DefaultController which is extended by FosRESTController. 
FosRESTBundle gives ability to change transfer state.
Default Controller allows to manipulate the view, form etc.   


##EventDispatcher

EventDispatcher component has been used in PrototypeBundle. Special service 'prototype.event', path: PrototypBundle/Event/Event is  responsible for this process. Workflow looks as below:
listener->dispatcher->event
DefaultController contains a dispatch() method.

##Create a simple app in a few steps
1. Define a major service file.
Prepare a service file, e.g. superadmin.services.yml. It could be empty only with parameters and services titles.
Add entry to the file: Config/DependencyInjection/CCOCallCenterExtnesion.
There are place for add a new entries, e.g.
```
$loader->load('superadmin.services.yml');

```
Where 'superadmin' is a name of application.

2. Define a main prefix of application.
#Resources/Config/routng.yml 
In the main routing add lines as below:
```
superadmin:
    resource: "@CCOCallCenterBundle/Resources/config/routing/superadmin.yml"
    prefix:   /superadmin
    type: be_simple_i18n 
```
  
3. Define routing file for application
Example based on superadmin app:
```
CCOCallCenterBundle/Resources/config/routing/superadmin.yml
```

Basic routings:
```
superadmin_callcenter_view:
    locales:  { en: "/{entityName}/{containerName}/{actionId}/view/{id}/{states}", pl: "/{entityName}/{containerName}/{actionId}/podglad/{id}/{states}"}
    defaults: { _controller: "prototype.grid_controller:viewAction", states: "", format: ~ }   
    requirements: {"containerName": main|container|element ,  "states": ".+"}
    options:
        expose: true 
        
superadmin_callcenter_new:
    locales:  { en: "/{entityName}/{containerName}/{actionId}/new/{states}", pl: "/{entityName}/{containerName}/nowy/{states}" }
    defaults: { _controller: "prototype.grid_controller:createAction", states: "", format: ~ }   
    requirements: {"containerName": main|container|element ,  "states": ".+"}
    options:
        expose: true      

superadmin_callcenter_list:
    locales:  { en: "/callcenter-list", pl: "/callcenter-lista"}
    defaults: { _controller: "prototype.grid_controller:listAction", states: "","entityName": "callcenter",  "containerName": "container", format: ~ }   
    requirements: {"containerName": main|container|element ,  "states": ".+"}
    options:
        expose: true 

```

On the top is a name of routing, `superadmin_callcenter_view`
In the locales is a route which is  after main prefix in the browser, '{ en: "/callcenter-list", pl: "/callcenter-lista"}', e.g.
'/superadmin/callcenter-list`
In defauls are parameters for recognize a corret action '_controller: "prototype.grid_controller:listAction"', correct entity '"entityName": "callcenter"' and view '"containerName": "container"'

4. Use a prototype command.
In this step prepare a needed files, e.g. listConfig, formType, twig files etc.
For begin this proccess use a command like below:
'prototype:generate' 
call this command example below:
```
CCOCallCenterBundle CCO\UserBundle\Entity\User Superadmin --withListConfig –-withListElementTwig

```
Description of parameters:
'CCOCallCenterBundle' - place where config is declared
'CCO\UserBundle\Entity\User' - name of entity
`Superadmin` - name of app and folder where generated files are located
'--withListConfig' - parameter if listConfig is needed
`–-withListElementTwig` - parameter if list twig is needed
Others parameters exist if  necessary.

6. Built services and configuration.
In the service file prepared befor, e.g. superadmin.services.yml add lines like below:
```
prameters:
 user.config.parameters:
        actions:    
            create:
                templates:
                    container: 'CCOCallCenterBundle:Superadmin\User\Container:list.html.twig' 
                    element: 'CCOCallCenterBundle:Superadmin\User\Element:list.html.twig'
            update:
                templates:
                    container: 'CCOCallCenterBundle:Superadmin\User\Container:list.html.twig' 
                    element: 'CCOCallCenterBundle:Superadmin\User\Element:list.html.twig'
        routings:
            read:
                route: superadmin_user_list
                route_params:
            create:
                route: superadmin_user_create
                route_params:

services:
    superadmin.user.dashboard.config:
        class: Core\PrototypeBundle\Service\Config
        arguments: [@prototype.config,%user.config.parameters%]        
        tags:
            -  { name: prototype.config, route: superadmin_user_list }  
    
    user.stream.listconfig:
        class: CCO\CallCenterBundle\Config\Superadmin\User\ListConfig
        arguments: [@service_container]        
        tags:
            -  { name: prototype.listconfig, route: superadmin_user_list}

```
In config service 'superadmin.user.dashboard.config' the second argument '%user.config.parameters%' is a name of parameters block.
In tags block route param is a name of route defined in superadmin.yml file.

Pamaters block and all structure is usefull when call in twig like below:
```
href={{ path(routeservice.getRouteName(config,'view'), buttonRouteParams)}}
```
There are other second posibiltiy to define a correct route wihtout parameters block.
If calling in twig looks like below:
```
href={{path(routePrefix~'_view',{"id":record.id, "containerName": "container", "actionId": "default",  "entityName": "callcenter"})}}
```
prepare services like descirbed at point no.3.





