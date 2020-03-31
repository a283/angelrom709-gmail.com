# angelrom709-gmail.com
database

Sistema CRUD Laravel / VueJS
Este trabajo esta hecho con en el framework Laravel, que se encuentra en las convenciones del framework por lo que encontraremos siguientes elementos:

Migraciones
Se  creo una migración para la tabla pages que contendra cada una de las paginas del sitio web:

Schema::create('pages', function (Blueprint $table) { $table->increments('id'); $table->string('title'); $table->string('slug'); $table->text('description'); $table->text('body'); $table->string('anterior')->nullable(); $table->string('siguiente')->nullable(); $table->timestamps(); });

Regularmente existen aquellos datos mínimos para que el sistema funciones, por ejemplo:

Catálogos por defecto
Datos de prueba
Usuario por defecto
En el proyecto actual, la clase PagesSeeder tiene el siguiente aspecto:

[ 'title' => 'Rom'S', 'slug' => str_slug('inicio'), 'description' => 'Esta constructura es una de las mejores en el pais', 'body' => 'Rom's es la empresa líder en construcción y operación de infraestructura en México, fundada en 1947 y con experiencia en trece países, siete sectores y un sólido portafolio de activos. <br> olucionamos los retos más complejos
de infraestructura con propuestas innovadoras,
rentables y sustentables. <br> Deseas construir, nosotros te ayudamos.', 'anterior' => '', 'siguiente' => 'acerca', 'created_at' => now() ],

Modelos
Por defecto se encuentra declarado el modelo User, ademas se ha creado el modelo Page que sera el encargado de manejar cada una de las páginas del sitio web, como ha sido declarado de acuerdo a las convenciones solo se han especificado los elementos rellenables con el siguiente arreglo:

protected $fillable = [ 'title', 'slug', 'description','body','anterior','siguiente' ];

Controlladores
En la parte de controladores solo se ha creado uno, PagesController, este sera el encargado de realizar el proceso CRUD de nuestro sistema, mismo que opera con verbos http y es aquí donde hacemos uso del ORM para gestionar los datos persistentes, como se muestra a continuación:

`public function update(Request $request, $slug){

$data=$request->input('data');

$page= Page::find($data['id']);

$page->title=$data['title']; $page->slug=$data['slug']; $page->description=$data['description']; $page->body=$data['body']; $page->anterior=$data['anterior']; $page->siguiente=$data['siguiente']; $page->save(); return $page; }`

Routes
La gestión de rutas se lleva a cabo por una parte con ayuda de Laravel, las rutas web se redireccionan a Vue Router mientras que para la gestión de recurso se utilizan las rutas api como se muestra a continuación: Route::get('pages/{slug}', 'PagesController@show'); Route::put('pages/{slug}', 'PagesController@update');

Por su parte el archivo de rutas de Vue tiene el siguiente aspecto: export default new Router({ routes:[ { path: '/', redirect: '/public/inicio' }, { path:'/public/:slug', name:'public', component:require('./views/seccion.vue').default, props:true },

Carga Asíncrona
La carga se manipula por medio de Axios de esta forma evitamos la carga completa de la página, la función para obtener los datos es la siguiente: axios.get(slug).then( res =>{ this.data = res.data })

Mejoras Futuras
Se espera corregir el nombre le la vista seccion a page para que coincida con la semántica, ademas, es indispensable restringir la vista edit/{slug} a solo usuarios con privilegios.
