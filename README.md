# Prototype architecture

## Services

Services are the major part of this bundle. 

Simple workflow of services could be include in one sentence :
**Contoller calls a service which is based on the routing.**
  1.  Service definition, path e.g.:PrototypeBundle\Resources\Config\services.yml:
```
  prototype.formtype:
        class: Core\PrototypeBundle\Form\FormType
        arguments: []        
        tags:
            -  { name: prototype.formtype } 

```

``` prototype.formtype ``` it is a name of service. It will be using by other classes for recognize a correct service. 
```class``` path t the service file
```arguments:[]``` an array of service arguments, could be null, container ```[@service_container] ``` etc.
```tags:  ``` element of correct service identify, could be a few, e.g.:
*    route - routing, only begining part is enough 
*    entity - full name of entity
*    actionId - there are two kinds, for routings and objects
*    *if name and route exist any other params are not neccesery


