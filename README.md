# Prototype architecture

## Services

Services are the major part of this bundle. 

Simple workflow of services could be include in one sentence :
**Contoller calls a service which is based on the routing.**
* Service definition, path e.g.:PrototypeBundle\Resources\Config\services.yml:
```
  prototype.formtype:
        class: Core\PrototypeBundle\Form\FormType
        arguments: []        
        tags:
            -  { name: prototype.formtype } 

```

  * First line: ``` prototype.formtype ``` it is a name of service. It will be using by other classes for recognize a correct service 
