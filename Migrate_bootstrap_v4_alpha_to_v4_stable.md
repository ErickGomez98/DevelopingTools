# Guía de modificaciones necesarias para realizar la migración de bootstrap v4-alpha a v4.1.1

## Archivos
- Agregar los archivos más recientes a la libreria de bootstrap
- Modificar el archivo del modulo de bootstrap library para que ahora cargue el popper.min.js en lugar de tether.min.js

#### Para buscar una clase o elemento para poder remplazarlo, usar la función de `grep -Ril "text-to-find" . --exclude-dir={docs*,vendor*}` en git bash

## Page
- Modificar `page.tpl.php` por este nuevo [template](https://github.com/ErickGomez98/DevelopingTools/blob/master/v4-page.tpl.php)
- Modificar las clases de hidden de los elementos de los menus de drupal, para no mostrar el link de salir en dispositivos grandes, remplazar las clases de hidden por las siguientes: `.d-block .d-sm-block .d-md-none`

## Alerts
- Sin cambios necesarios

## Badge
- Cambiar `.badge-default` por `.badge-secondary`

## Buttons
- Sin cambios necesarios

## Button Group
- Sin cambios necesarios

## Card
- Eliminar la clase card-block ya que no es necesaria
- Agregar el `div.card-block` a las cards que no lo tengan, algunos div's tienen la clase de `card-block`, básicamente hay que sustituir esa clase por `card-body`
- Las cards que tengan la clase `card-outline-*color*` se eliminará esa clase y se remplazará por la siguiente: `card-outline-*color*` por ejemplo `card-outline-success` para darle el efecto de outline

## Collapse
- Sin cambios necesarios

## Forms
#### Custom Radios
  - La estructura en general cambio, antes todo estaba agrupado dentro de un `<label>`, ahora todo está agrupado de la siguiente manera en un `<div>`:
  ```html
  <div class="custom-control custom-radio custom-control-inline">
    <input type="radio" id="customRadioInline1" name="customRadioInline1" class="custom-control-input">
    <label class="custom-control-label" for="customRadioInline1">Toggle this custom radio</label>
  </div>
  ```
  - Entonces, aparte cuando el formulario sea inline, deberá quedar agrupado dentro de otro `<div>` de la siguiente manera:
  ```html
  <div class="form-check form-check-inline">
      <div class="custom-control custom-radio custom-control-inline">
          <input name="radio_tipo_negocio_cliente" id="customRadioInline1" type="radio"
                 class="select_tipo_negocio_cliente custom-control-input" value="2">
          <label class="custom-control-label" for="customRadioInline1">Particular</label>
      </div>
  </div>
  ```
  
### Custom Checkbox
  - La estructura es muy parecida al custom radio, a diferencia de una clase y el tipo de input, entonces un checkbox inline quedaria de la siguiente manera:
  ```html
  <div class="form-check form-check-inline">
      <div class="custom-control custom-checkbox custom-control-inline">
          <input class="custom-control-input" type="checkbox"
                 value="1" data-validation="checkbox_group" id="customCheckInline1" name="client_areas[]"
                 data-validation-qty="min1" data-validation-error-msg="Selecciona al menos una área.">
          <label class="custom-control-label" for="customCheckInline1">CCTV</label>
      </div>
  </div>

  ```
## List Group
- Sin cambios necesarios
  







