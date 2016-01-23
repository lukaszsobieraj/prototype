# Prototype architecture

## Services

Services are the major part of this bundle. 

Simple workflow of services could be include in one sentence :
**Contoller calls a service which is based on the routing.**
  1.  Service definition, location e.g.:PrototypeBundle\Resources\Config\services.yml:
```
  prototype.formtype:
        class: Core\PrototypeBundle\Form\FormType
        arguments: []        
        tags:
            -  { name: prototype.formtype } 

```

``` prototype.formtype ``` it is a name of service. It will be using by other classes for recognize a correct service. 
```class``` path to the service file
```arguments:[]``` an array of service arguments, could be null, container ```[@service_container] ``` etc.
```tags:  ``` element of correct service identify, could be a few, e.g.:
*    route - routing, only begining part is enough 
*    entity - full name of entity
*    actionId - there are two kinds, for routings and objects
*   **Attention:** if name and route exist any other params are not neccesery
  2. Commands
Prototype bundle were prepared with a  special commands for easiest work proccesss. The solution is based on a generators. The solution is  hide in authomatic. The specific commands allow to build almost every type of important file, e.g FormType etc. 
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
  1.
Basic configuration location: PrototypeBundle\DependencyInjection\Configuration
This is build on a tree structure with TreeBuilder object. Default file always appears after generate new bundle.  Includes information about basic elements of application, e.g. twigs. Could be overwritten by Controller(application), Service.
