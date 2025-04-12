<!-- MULTILANGUAJE MENU START -->
ES | [EN](https://lckpig.gitbook.io/practical-dev-handbook/css/optimization/minification-and-compression)
<!-- MULTILANGUAJE MENU END -->

# Minificación y Compresión de CSS

La minificación y compresión de CSS son técnicas fundamentales para reducir el tamaño de los archivos CSS, mejorando así los tiempos de carga y el rendimiento general del sitio web.

## Minificación de CSS

La minificación reduce el tamaño del archivo CSS eliminando caracteres innecesarios sin alterar su funcionalidad.

### ¿Qué elimina la minificación?

- Espacios en blanco y tabulaciones
- Saltos de línea
- Comentarios
- Puntos y coma finales (cuando es posible)
- Ceros innecesarios (ej. 0.5s → .5s)
- Comillas innecesarias

### Ejemplo

**CSS Original:**
```css
/* Estilos para botones */
.button {
  color: #333333;
  background-color: #FFFFFF;
  padding: 10px 20px;
  border-radius: 5px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
  transition: all 0.3s ease;
}

.button:hover {
  background-color: #F5F5F5;
}
```

**CSS Minificado:**
```css
.button{color:#333;background-color:#fff;padding:10px 20px;border-radius:5px;box-shadow:0 2px 4px rgba(0,0,0,.2);transition:all .3s ease}.button:hover{background-color:#f5f5f5}
```

### Herramientas de minificación

- **Online**: [CSS Minifier](https://cssminifier.com/), [Clean CSS](https://www.cleancss.com/css-minify/)
- **Node.js**: Clean-CSS, cssnano, UglifyCSS
- **Gulp/Webpack**: gulp-clean-css, optimize-css-assets-webpack-plugin
- **Build tools**: La mayoría de herramientas de build (Vite, Create React App, etc.) incluyen minificación por defecto

## Compresión de archivos

La compresión reduce aún más el tamaño de los archivos CSS mediante algoritmos que compactan la información.

### Tipos comunes de compresión

- **Gzip**: Compresión estándar de web servers, reduce tamaño ~70-80%
- **Brotli**: Algoritmo más moderno que ofrece mejor relación de compresión que Gzip

### Configuración de compresión

**Apache (.htaccess):**
```
<IfModule mod_deflate.c>
  # Comprimir recursos de texto
  AddOutputFilterByType DEFLATE text/css
  
  # Nivel de compresión (1-9, donde 9 es máxima compresión)
  DeflateCompressionLevel 9
</IfModule>
```

**Nginx (nginx.conf):**
```
gzip on;
gzip_comp_level 6;
gzip_min_length 256;
gzip_types text/css;
```

## Optimización adicional de CSS

### Purging CSS

Elimina CSS no utilizado en tu aplicación:

- **PurgeCSS**: Analiza HTML y JS para eliminar clases CSS no utilizadas
- **UnCSS**: Similar a PurgeCSS, elimina estilos no usados

### Subset de fuentes

Si usas @font-face, asegúrate de incluir solo los caracteres necesarios:

```css
@font-face {
  font-family: 'MiFuente';
  src: url('mifuente-subset.woff2') format('woff2');
  unicode-range: U+0000-00FF, U+0131; /* Sólo caracteres latinos básicos */
  font-display: swap;
}
```

## Impacto en el rendimiento

| Técnica           | Reducción típica              | Esfuerzo de implementación    |
| ----------------- | ----------------------------- | ----------------------------- |
| Minificación      | 15-25%                        | Muy bajo (automatizable)      |
| Compresión Gzip   | 70-80% adicional              | Bajo (configuración servidor) |
| Compresión Brotli | 15-20% más que Gzip           | Medio (menos soporte)         |
| Purging CSS       | 50-90% (depende del proyecto) | Medio                         |

La combinación de minificación y compresión puede reducir drásticamente el tamaño de los archivos CSS, mejorando significativamente los tiempos de carga, especialmente en conexiones lentas o dispositivos móviles. 