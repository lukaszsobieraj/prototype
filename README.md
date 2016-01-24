# Prototype architecture

## Services

PrototypeBundle was built on the microservice architecture pattern. 

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
    c)    arguments:[] -  an array of service arguments, could be null, container ```[@service_container] ``` etc.
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
  1. Basic configuration location: PrototypeBundle\DependencyInjection\Configuration
This is build on a tree structure with TreeBuilder object. Default file always appears after generate new bundle.  Includes information about basic elements of application, e.g. twigs. Could be overwritten by Controller(application), Service.


  2. Services configuration location: PrototypeBundle\Services\Config.
  Main pattern for create this kind of configuration looks like example below:

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

First part of the structure includes name of controller action e.g. read, list, new etc. Templates are the main twigs with typical content depends on view type, element or container. Structure of routings has been built on the same structure as actions, with one additional route params.
