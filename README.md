# Repositorio plantilla basado en Generic.

Este repositorio contiene los archivos que pueden ser usados como "plantilla" para hacer funcionar el estilo "clon" de Generic. Están creados mediante el nuevo método de creación de estilos, por lo que hay muchos elementos *minificados* o ilegibles; no obstante, de esos elementos no os tendréis que ni preocupar. A continuación os dejo los pasos necesarios para poner el nuevo estilo en marcha.

## Clonar el repositorio.
He establecido que el repositorio sea un "repositorio plantilla", es decir, que no tendréis que copiar a mano los archivos, sino que se creará un repositorio que será una copia literal de este, pero asociado a vuestro usuario. Para crear el repositorio, seguid [esta guía](https://help.github.com/es/github/creating-cloning-and-archiving-repositories/creating-a-repository-from-a-template).

## Modificar los archivos para el correcto funcionamiento.
Antes de empezar a trabar, tendréis que modificar los siguientes archivos para que todo funcione correctamente.

### Archivo _style.js_.
En este archivo tendréis que sustituir, como siempre, el nombre del estilo a crear. Voy a poner un ejemplo para ver esto. Suponiendo un estilo de nombre _Estilo Guay_:

#### Creamos los nombres del estilo.
Como sabéis, el nombre del estilo en algunos sitios va con mayúsculas y en otros solamente en minúsculas y con guiones. Vamos a crear los nombres en código de nuestro nuevo estilo de ejemplo:

- el nombre del nuevo estilo en mayúsculas será **_EstiloGuay_**, es decir, todo junto con las palabras separadas por espacios en mayúscula. Si no hay espacios, solamente habrá una mayúscula.
- por otro lado, el nombre del estilo en minúsculas será **_estilo-guay_**, es decir, todo en minúsculas, y sustituyuendo los espacios por guiones. Si no hay espacios no habrá guiones.

#### Reemplazamos los nombres creados.
Vale, tenemos los nombres, pero ahora ¿Dónde va cada uno? Estos son los textos a ser sustituidos por los nombres que acabamos de crear:

- donde el nombre contenga **mayúsculas**, el texto a sobreescribir será **_Generic_**. En el ejemplo, sustituiríamos **_Generic_** por **_EstiloGuay_**.
- donde el nombre vaya **solo en minúsculas**, el texto a sobreescribir será **_generic_**. De nuevo en el ejemplo, sustituiríamos el texto **_generic_** por **_estilo-guay_**.

**¡IMPORTANTE!** |
-----------------|
Tened muy en cuenta que, si sobreescribis la palabra **generic** sin diferenciar mayúsculas de minúsculas, se sustituirán ambos casos **tanto _generic_ como _Generic_**.|

####  Ejemplo de resultado.
Si todo ha ido bien, pasaríamos de tener esto:
```
GenericStyle.prototype = {
	parent: blink.theme.styles.basic.prototype,
	bodyClassName: 'content_type_clase_generic',
	extraPlugins: ['image2'],
	ckEditorStyles: {
		name: 'generic',

// etc...
```
a tener esto:
```
EstiloGuayStyle.prototype = {
	parent: blink.theme.styles.basic.prototype,
	bodyClassName: 'content_type_clase_estilo-guay',
	extraPlugins: ['image2'],
	ckEditorStyles: {
		name: 'estilo-guay',

// etc...
```
Llegados a este punto ya estaría todo terminado. Recomiendo buscar los strings de texto _Generic_ y _generic_ por si hubiera habido algún despiste al sobreescribir.

## Uso de los archivos.
Una vez configurado todo, y comprobado que el estilo se importa correctamente con la url de github, podréis añadir lo que queráis.

### Archivo _style.js_.
En este archivo, como lo único que interesa modificar son los estilos de CKEditor, solamente tendréis que buscar esta parte:

```
/*
*   Array con la definición de los estilos para el editor de CKEditor
*/
const ckeStyles = [
	{name: 'Caja 1', type: 'widget', widget: 'blink_box', attributes: {'class': 'box-1'}},
	{name: 'Caja 2', type: 'widget', widget: 'blink_box', attributes: {'class': 'box-2'}},
	{name: 'Énfasis', element: 'span', attributes: {'class': 'bck-enfasis'}}
	// Añadir elementos CKEditor aquí.
];
```

Ahí es donde podréis meter los estilos que queráis.

## Archivo CSS.
Este archivo es un poco "caótico", consecuencia del proceso de transformación que aplica Webpack (que es la utilidad que usamos para crear los nuevos estilos); no obstante, no tenéis que tocar nada realmente del CSS existente.

Con que añadáis el CSS al final, en la parte indicada, sería suficiente:

```
  /************************
   * CSS AÑADIDO POR HTML *
   ************************/

		/* Vuestro CSS aquí. */

   /************************
   * CSS AÑADIDO POR HTML *
   ************************/
```

De esta forma, tendréis más localizados los cambios que habéis añadido.

## Algunos apuntes.

### Fragilidad del código.
Mucho cuidado a la hora de tocar los archivos. Al estar preparados para funcionar de la forma más comprimida posible, cualquier modificación puede afectar al funcionamiento, así que cuidado a borrar alguna línea o carácter del código que no sea el que se ha especificado, porque puede que os deje de funcionar.

### Guerra de selectores.
¡Ojo! Quizá por un tema de selectores, habrá propiedades que no se os apliquen; por ejemplo, si ponéis una regla de estilo tal como:

```
#layout-container div[id*="section-screen"] .content-img {
  background-color: #3A462C;
}
```

Es posible que *no se aplique* porque en el archivo hay una regla más "específica" que asigna la misma propiedad CSS (background-color):

```
#layout-container div[id*="section-screen"] .section .section-content .tabs-content .content.teacher-content .unit-content .content-item .content-img {
  background-color: pink;
}
```

En este caso, tendréis que crear un selector más específico aún; siguiendo con el ejemplo, si copiamos el selector original y le añadimos un `body` al selector, seguramente ya sí que funcionaría:
```
body #layout-container div[id*="section-screen"] .section .section-content .tabs-content .content.teacher-content .unit-content .content-item .content-img {
  background-color: #3A462C;
}
```
