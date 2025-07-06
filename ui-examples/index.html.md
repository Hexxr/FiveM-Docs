# Index.html

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Exo+2:wght@300;400;500;600;700&display=swap" rel="stylesheet" />

    <!-- Quasar CSS Framework -->
    <link href="https://cdn.jsdelivr.net/npm/quasar@2.12.0/dist/quasar.prod.css" rel="stylesheet" type="text/css" />

    <!-- Font Awesome -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" />

    <!-- Roboto Font and Material Icons -->
    <link href="https://fonts.googleapis.com/css?family=Roboto:100,300,400,500,700,900|Material+Icons" rel="stylesheet" type="text/css" />

    <!-- Custom Styles -->
    <link href="css/style.css" rel="stylesheet" />
    <link rel="stylesheet" href="css/drawtext.css" />

    <!-- Vue.js and Quasar Scripts -->
    <script src="https://cdn.jsdelivr.net/npm/vue@3/dist/vue.global.prod.js" defer></script>
    <script src="https://cdn.jsdelivr.net/npm/quasar@2.12.0/dist/quasar.umd.prod.js" defer></script>

    <!-- Application Scripts -->
    <script type="module" src="js/app.js"></script>
    <script src="js/drawtext.js"></script>
  </head>

  <body>
    <!-- Main Application Container -->
    <div id="q-app" style="min-height: 100vh"></div>

    <!-- Draw Text Container -->
    <div id="drawtext-container">
      <div id="text" class="text"></div>
    </div>
  </body>
</html>
```
