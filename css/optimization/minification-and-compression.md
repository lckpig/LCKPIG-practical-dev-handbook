<!-- MULTILANGUAJE MENU START -->
EN | [ES](https://lckpig.gitbook.io/es-practical-dev-handbook/css/optimization/minification-and-compression)
<!-- MULTILANGUAJE MENU END -->

# CSS Minification and Compression

CSS minification and compression are fundamental techniques for reducing the size of CSS files, thus improving loading times and overall site performance.

## CSS Minification

Minification reduces the file size by removing unnecessary characters without altering functionality.

### What Minification Removes

- Whitespace and tabs
- Line breaks
- Comments
- Final semicolons (when possible)
- Unnecessary zeros (e.g., 0.5s → .5s)
- Unnecessary quotes

### Example

**Original CSS:**
```css
/* Button styles */
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

**Minified CSS:**
```css
.button{color:#333;background-color:#fff;padding:10px 20px;border-radius:5px;box-shadow:0 2px 4px rgba(0,0,0,.2);transition:all .3s ease}.button:hover{background-color:#f5f5f5}
```

### Minification Tools

- **Online**: [CSS Minifier](https://cssminifier.com/), [Clean CSS](https://www.cleancss.com/css-minify/)
- **Node.js**: Clean-CSS, cssnano, UglifyCSS
- **Gulp/Webpack**: gulp-clean-css, optimize-css-assets-webpack-plugin
- **Build tools**: Most build tools (Vite, Create React App, etc.) include minification by default

## File Compression

Compression further reduces file size through algorithms that compact the information.

### Common Compression Types

- **Gzip**: Standard web server compression, reduces size by ~70-80%
- **Brotli**: More modern algorithm offering better compression ratio than Gzip

### Compression Configuration

**Apache (.htaccess):**
```
<IfModule mod_deflate.c>
  # Compress text resources
  AddOutputFilterByType DEFLATE text/css
  
  # Compression level (1-9, where 9 is maximum compression)
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

## Additional CSS Optimization

### Purging CSS

Removes unused CSS in your application:

- **PurgeCSS**: Analyzes HTML and JS to remove unused CSS classes
- **UnCSS**: Similar to PurgeCSS, removes unused styles

### Font Subsetting

If using @font-face, make sure to include only necessary characters:

```css
@font-face {
  font-family: 'MyFont';
  src: url('myfont-subset.woff2') format('woff2');
  unicode-range: U+0000-00FF, U+0131; /* Only basic Latin characters */
  font-display: swap;
}
```

## Performance Impact

| Technique          | Typical Reduction          | Implementation Effort      |
| ------------------ | -------------------------- | -------------------------- |
| Minification       | 15-25%                     | Very low (automatable)     |
| Gzip Compression   | 70-80% additional          | Low (server configuration) |
| Brotli Compression | 15-20% more than Gzip      | Medium (less support)      |
| Purging CSS        | 50-90% (project dependent) | Medium                     |

The combination of minification and compression can dramatically reduce CSS file sizes, significantly improving loading times, especially on slow connections or mobile devices. 