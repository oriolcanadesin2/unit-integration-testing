# Tests Unitarios y de Integración con Spring Boot

## Diferencia entre Test Unitario y Test de Integración

### Test Unitario

* **Propósito**: Verificar el correcto funcionamiento de una parte específica del código, generalmente un método o una clase.

* **Aislamiento**: Se realiza en aislamiento del resto del sistema. Las dependencias externas como bases de datos, sistemas de archivos, etc., suelen ser reemplazadas por mocks o stubs.

* **Rapidez y simplicidad**: Son generalmente rápidos y sencillos de escribir y ejecutar.

```java
import org.junit.jupiter.api.Test;
import static org.mockito.Mockito.*;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
public class MiServicioUnitTest {

    @Test
    public void testMiMetodo() {
        MiRepositorio mockRepositorio = mock(MiRepositorio.class);
        MiServicio miServicio = new MiServicio(mockRepositorio);

        // Supuestos y lógica del test
        // ...

        verify(mockRepositorio).metodoEspecifico();
    }
}
```

### Test de Integración

* **Propósito**: Verificar cómo diferentes módulos o servicios funcionan juntos. Se enfoca en las interfaces y flujos de datos entre los módulos.

* **Integración con sistemas reales**: A menudo involucra la integración con bases de datos, sistemas de archivos, y otros servicios externos.

* **Complejidad y tiempo de ejecución**: Suelen ser más complejos y tardan más en ejecutarse que los tests unitarios.

```java
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.reactive.WebFluxTest;
import org.springframework.test.web.reactive.server.WebTestClient;

@WebFluxTest
public class MiControladorIntegrationTest {

    @Autowired
    private WebTestClient webTestClient;

    @Test
    public void testEndpoint() {
        webTestClient.get().uri("/mi-endpoint")
            .exchange()
            .expectStatus().isOk()
            .expectBody(String.class).isEqualTo("Respuesta esperada");
    }
}
```

## ¿Cómo realizar tests de integración de forma correcta?

Como hemos visto, los tests de integración realizan peticiones y llamadas reales, es decir, que se comunican con otros servicios sin utilizar mocks o stubs. Esto puede ser un problema si no se realiza de forma correcta, ya que puede afectar a la integridad de los datos de los sistemas reales.

Para evitar esto, Spring Boot nos ofrece la posibilidad de utilizar un contexto de Spring Boot específico para los tests de integración, que nos permite configurar el entorno de ejecución de los tests de forma independiente al entorno de producción.

