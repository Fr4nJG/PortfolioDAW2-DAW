## 1. Javadoc (Java)

Herramienta estándar para documentar código Java. Viene incluida en el JDK y lee comentarios `/** ... */` con etiquetas como `@param`, `@return` y `@throws`.
```java
/**
 * Programa de ejemplo que muestra "Hello, World!" por consola.
 */
public class HelloWorld {

    /**
     * Punto de entrada del programa.
     *
     * @param args argumentos de línea de comandos
     */
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

- **Facilidad de uso:** muy fácil. No requiere instalación aparte y los IDE (IntelliJ, Eclipse) la soportan de forma nativa.
- **Integración con proyectos:** se ejecuta desde Maven (`maven-javadoc-plugin`) o Gradle y encaja bien en cualquier pipeline de CI.
- **Formatos de salida:** HTML. El Markdown en comentarios está disponible desde JDK 23. El PDF solo se consigue con herramientas externas.
- **Comunidad:** muy grande; es el estándar de facto en el ecosistema Java.
- **Curva de aprendizaje:** baja.
- **Limitaciones:** solo sirve para Java y su aspecto es poco personalizable.

## 2. JSDoc (JavaScript)

Genera documentación HTML a partir de comentarios con etiquetas en código JavaScript.

```javascript
/**
 * @file Programa de ejemplo que muestra "Hello, World!" por consola.
 */

console.log("Hello, World!");
```

- **Facilidad de uso:** fácil. Se instala con npm y requiere poca configuración.
- **Integración con proyectos:** funciona con scripts de npm y CI. Para proyectos en TypeScript conviene usar TypeDoc.
- **Formatos de salida:** HTML. Existen herramientas como `jsdoc-to-markdown` para generar Markdown, y el PDF requiere soluciones externas.
- **Comunidad:** grande, aunque la herramienta recibe menos actividad que otras del mismo tipo.
- **Curva de aprendizaje:** baja.
- **Limitaciones:** el aspecto por defecto es básico y no cubre bien los proyectos en TypeScript.



## 3. Sphinx (Python)

Herramienta de referencia en el mundo Python para documentación técnica extensa. Combina docstrings (mediante la extensión `autodoc`) con archivos reStructuredText o MyST (Markdown).
```python
"""Programa de ejemplo que muestra "Hello, World!" por consola."""

print("Hello, World!")
```

- **Facilidad de uso:** media. Hay que configurar `conf.py` y conocer reStructuredText.
- **Integración con proyectos:** se integra con Read the Docs, GitHub Actions y un gran número de extensiones.
- **Formatos de salida:** HTML, PDF (vía LaTeX), ePub y páginas man.
- **Comunidad:** muy grande; es el estándar en Python.
- **Curva de aprendizaje:** media.
- **Limitaciones:** reStructuredText resulta menos intuitivo que Markdown y la generación de PDF exige tener LaTeX instalado.




**Facilidad de uso**
- Javadoc: muy fácil.
- JSDoc: fácil.
- Sphinx: media.

**Integración con proyectos**
- Javadoc: Maven, Gradle e IDE.
- JSDoc: npm scripts y CI.
- Sphinx: Read the Docs, GitHub Actions y extensiones.

**Formatos de salida**
- Javadoc: HTML (Markdown en comentarios desde JDK 23).
- JSDoc: HTML (Markdown con herramientas adicionales).
- Sphinx: HTML, PDF, ePub y man.

**Comunidad**
- Javadoc: muy grande.
- JSDoc: grande.
- Sphinx: muy grande.

**Curva de aprendizaje**
- Javadoc: baja.
- JSDoc: baja.
- Sphinx: media.


## Conclusiones y recomendación

- **Javadoc** es la opción natural para proyectos Java: sin instalación, integrada y con una curva de aprendizaje mínima.
- **JSDoc** es la mejor opción rápida para JavaScript, con una sintaxis sencilla, aunque limitada en formatos y estética.
- **Sphinx** es la más potente de las tres: ofrece más formatos de salida (incluido PDF) y permite escribir documentación extensa, a cambio de una configuración inicial mayor.

> Nota: los datos proceden de conocimiento general, no de una consulta reciente. Conviene verificar versiones y novedades (por ejemplo, el soporte Markdown de Javadoc) en la documentación oficial antes de entregar la ficha.
