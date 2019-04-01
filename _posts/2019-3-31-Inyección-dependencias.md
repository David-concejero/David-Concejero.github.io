## Introdución

Partiendo de:

• Previa explicación y pruebas en proyectos con uso de interfaces
• Ejemplos Varios: Usando springframework

Se pide

```sh

(1) ¿Qué entendiste? Demuestra que tienes claro el tema.
Explicalo de tal forma que cualquiera pueda entenderlo,
mencionando también los beneficios de usarlo.
```

## ¿Que es la inyección de dependencias?

Cuando escribimos una aplicación en Java lo ideal sería hacer que nuestras clases fuesen lo más independientes posibles, esto nos permitiría poder reutilizarlas y probarlas en otro tipo de clases sin ningun tipo de problema. Aquí es donde toma el papel nuestro framework.

Spring-Framework evita la problematica de que nuestras clases tengan que instanciar una a una todos los objetos que necesiten, ya que el se encargará de inyectar dichos objetos cuando una clase lo necesite. En este momento entran en juego 2 conceptos que no debemos confundir el uno con el otro. El DI y el IOC

• **IOC** (Inversión de Control): Este concepto hace referencia a los diseños donde el flujo de ejecución de un programa se invierte. Es decir, un framework o librería controla el flujo de nuestro programa. 

• **DI** (Inyección de dependencias): Y por el contrario, este concepto hace referencia a un diseño donde intentamos separar el proceso de construcción de la ejecución. Es decir, la idea de este diseño es la de suministrar los objetos a una clase en lugar de que la clase sea quien cree dichos objetos. 

## La inyección de dependencias involucra 4 roles:

  • Objetos de servicios a utilizar

  • Objeto cliente

  • La interfaz para definir como los clientes usarán el servicio

  • El inyector que es responsable de construir los servicios e inyectarlos en el 
    cliente

## Y hay tres tipos de inyección de dependencias:

Veamos un ejemplo:


**Mediante un constructor**
```sh
Client(Service service) {
    this.service = service;
}
```

**Mediante un método Setter**
```sh
public void setService(Service service) {
    this.service = service;
}
```

**A través de una Interfaz**
```sh
// Interfaz servicio Setter.
public interface ServiceSetter {
    public void setService(Service service);
}

// Clase cliente 
public class Client implements ServiceSetter {
    private Service service;

    @Override
    public void setService(Service service) {
        this.service = service;
    }
}
```

## Y vamos a ver para acabar un ejemplo usando el spring-framework

```sh
public class Injector {
  public static void main(String[] args) {
    BeanFactory beanfactory = new ClassPathXmlApplicationContext("Beans.xml");
    Client client = (Client) beanfactory.getBean("client");
    System.out.println(client.greet());
  }
}

 <?xml version="1.0" encoding="UTF-8"?>
 <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">

    <bean id="service" class="ExampleService">
    </bean>

    <bean id="client" class="Client">
        <constructor-arg value="service" />        
    </bean>
</beans>
```
## Beneficios de la inyección de dependencias

  •  Ganas flexibilidad en tu programa al no estar ligado a otras clases

  •  Es perfecto para realizar 'tests' sobre tu código, ya que puedes inyectar implementaciones simuladas de estas dependencias

  •  Reduce el código de cada componente de nuestro proyecto, lo que nos permite identificar a simple vista cada dependencia.
  
  •  Reduces al máximo los componentes innecesarios del proyecto
