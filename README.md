# Prototype architecture

## Services

Services are the major part of this bundle. 
Workflow services :
**Contoller calls a service which is based on the routing.**
Service definition looks like example below:
```
  prototype.formtype:
        class: Core\PrototypeBundle\Form\FormType
        arguments: []        
        tags:
            -  { name: prototype.formtype } 

```

