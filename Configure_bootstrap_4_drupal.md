## Pasos para adaptar un drupal a que funcione con bootstrap 4
1. Descargar, instalar y habilitar el tema de drupal de bootstrap. 
2. Descargar e instalar la versión más reciente del módulo [Bootstrap Library](https://www.drupal.org/project/bootstrap_library).
2. Habilitar el módulo Bootstrap Library.
3. Ir a `admin/config/development/bootstrap_library` y en el apartado de "Theme visibility" marcar la opción "Only the listed themes" y seleccionar el tema del sistema interno.
4. Agregar los archivos de la libreria de bootstrap 4 a `/sites/all/libraries/bootstrap`
5. En el tema del sistema interno, ir a `admin/appearance/settings/~theme~` y seleccionar CDN Provider = None
6. Agregar `'js/tether' . $min . '.js'` en el `$js_array` de la `function _bootstrap_library_get_files` en `/sites/all/modules/bootstrap_library/bootstrap_library.module` (Deberá de quedar antes del archivo de bootstrap para que cargue primero).
7. Agregar `$attributes['class'][] = 'nav-item';` dentro de la función `bootstrap_menu_link` en `/sites/all/themes/bootstrap/templates/menu/menu-link.func.php`
8. Agregar la función de `l_custom` que se quedo en partnersinterno al archivo de bootstrap `/sites/all/themes/bootstrap/templates/menu/menu-link.func.php` 
9. Configurar el siguiente archivo a como está el de PcPartners
`/sites/all/themes/bootstrap/templates/menu/menu-tree.func.php`
9. Agregar el siguiente archivo [page.tpl.php](page.tpl.php) dentro de la carpeta de templates del módulo del sistema interno.
10. El sistema deberia de estar funcionando correctamente en bootstrap v4 sin ningún problema.
