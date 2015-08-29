#Backbone validator v0.5
###Mauricio Manjarrez @wealthymaury

Plugin para validar modelos en Backbone.js

Desarrollado y probando con las siguientes versiones:

* Backbone.js v1.2.2
* Underscore.js v1.8.3


INTRODUCCION:

Este plugin es una manera facil de realizar validaciones a los modelos de Backbone
sin necesidad de escribir toda la logica que eso conlleva, utilizando una sintaxis
inspirada en el framework backend Laravel.. Con la flexibilidad necesaria para realizar
validaciones mas complejas que no sean soportadas por el plugin...

COMO INICIAR:

1. Descarga cualquiera de las versiones de produccion del plugin:
	
	* con errores en Español: [backbone.validator.min.js](https://raw.githubusercontent.com/Wealthymaury/backbone.validator/master/dist/es/backbone.validator.min.js) *10kb*
	* con errores en Ingles: [backbone.validator.min.js](https://raw.githubusercontent.com/Wealthymaury/backbone.validator/master/dist/en/backbone.validator.min.js) *10kb*

2. Incluye el archivo en tu pagina HTML despues de Backbone.js y sus dependencias
	```html
	- <script src="backbone.validator.min.js"></script>
	```

3. Haz la declaracion de tus modelos siguiendo las siguientes reglas:
	- Para utilizar la validacion de backbone.validator, debes incluir una funcion en tu modelo
	  con el nombre 'validatorRules', la cual debe tener la siguiente estructura:
	```js
	  validatorRules : {
			campo1 	: 'reglas',
			campo2 	: 'reglas',
			campoN 	: 'reglas',
	  }
	```

	  Donde:
	  
	  	campoN -> campo del modelo a aplicar las reglas
	  	
	  	reglas -> deben tener el siguiente formato 'regla1|regla2|reglaN:arg1,arg2|...'
	  
	  Ejemplo:
	  ```js
	  var M = Backbone.Model.extend(
	  {   urlRoot: '',
		  validatorRules : {
				name 	: 'required|string|minLength:2|maxLength:10',
				age 	: 'required|integer|between:15,99',
				active 	: 'required|boolean',
				type 	: 'required|in:admin,empl'
		  }
	  });
	  ```

4. Las reglas que se pueden usar son las siguientes:
	```js	
    REGLA			[OPCIONES] 							| 	EJEMPLO
	--------------------------------------------------------------------------
	- required
	- string
	- integer
	- in 			[lista de values(separados por ,)]  |	in:1,2,3
	- not_in 		[lista de values(separados por ,)]  |	not_in:1,2,3
	- array
	- min		    [value integer]						|	min:1
	- minLength		[value integer]	    				|	minLegth:1
	- max			[value integer]						|	max:2
	- maxLength  	[value integer]	    				|	maxLegth:1
	- between	  	[2 valores separados por ,] 		|	between:1,5
	- betweenLength	[2 valores separados por ,] 		|	betweenLength:1,5
	- length 		[valor]								| 	length:5
	- same	 		[campo del modelo a evaluar] 		|	same:field
	- different 	[campo del modelo a evaluar] 		|	different:field
	- confirmed 	[evalua que exista un campo que se llame igual pero con terminacion _confirmed y que tenga el mismo valor]										
	- boolean
	- email
	- mimes 		[atributos] 						|  	mimes:png,jpg,jpeg
	- date  											
	- before 		[atributo fecha] 					|  	before:2015-08-03
	- after 		[atributo fecha] 					|  	after:2015-08-03
	```

5. Si deseas hacer validaciones adicionales puedes implementar el siguiente metodo y ahi escribirlas:
	```js
	moreValidations : function(model, options)
	{ 	
		//la logica tu la recides, pero...
		//deberas retornar los errores en el siguiente formato
	
		var errors = {
			field1: [
				'Error1',
				'Error2',
			],
			field2: [
				'Error1',
				'Error2',
			]
		};
		
		return errors;
	}
	```
	
6. Una vez escrito tu modelo, haz la instancia y ejecuta model.isValid(), funciona como normalmente lo haria

7. Para acceder a los errores es con model.validationError

8. hay algunos mensajes que usan el nombre del campo en el modelo, si deseas que el nombre aparezca a tu manera
   solo agregalo a Validator.Lang['ex-mx|en-us'].fields asi:
	```js
   Validator.Lang['en-us'].fields.name = 'Name';
   Validator.Lang['es-mx'].fields.name = 'Name';
   Validator.Lang['en-us'].fields.age = 'Age';
   Validator.Lang['es-mx'].fields.age = 'Edad';
   ```

   Lo que agregues sera usado donde se requiera...
   
9. Si lo deseas, puedes crear tus propias reglas de validacion que se apeguen a la sintaxis del plugin, para ello realiza lo siguiente:
   
   * debes generar una funcion dentro del modelo que tenga el nombre con el que vaz a conocer la regla,
   * dicha funcion recide como primer parametro el nombre del campo que se esta validando, 
   * y en caso de que quieras pasarle argumentos, se reciben a continuacion del 'field_name':
   
    
    ```js
    miNuevaRegla: function(field_name, arg1, arg2)
    {
    	var value = this.get(field_name);
    	
    	//aqui haz la logica que quieras, pero en caso de error debes retornar una cadena...
    	
    	return 'Mi mensaje de error para la validacion..';
    }
    ```
    
   
    * La manera de usar tu regla, para este ejemplo seria asi:
    
   
    ```js
    	validatorRules: {
    	   name: 'miNuevaRegla:arg1,arg2'
    	}
    ```




