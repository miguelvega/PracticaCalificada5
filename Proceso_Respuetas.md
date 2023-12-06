# Proceso y Respuestas

En primera instancia ejecutamos la migración para aplicar los cambios a la base de datos, para ellos debemos ejecutar el comando 
rake db:migrate. Sin embargo ejecutar el comando bin/rails server notamos que la base de datos actual esta vacia debido a que 
no hemos incorporado las semillas dadas en el archivo seeds.rb. Por ello ejecutamos rake db:seed.

![Captura de pantalla de 2023-12-06 07-38-37](https://github.com/miguelvega/PracticaCalificada5/assets/124398378/cb2c79b4-c7b2-48b2-b930-bedd76a84a11)

### Preguntas:  En este conjunto de preguntas tus respuestas deben ir de acuerdo a las actividades correspondientes, no se puntúa sino hay evidencia del uso de los scripts desarrollados y solo presentas respuestas sin evidencia de lo desarrollado a lo largo del curso. (7 puntos)

1. En las actividades relacionados a la Introducción de Rails los métodos actuales del controlador no son muy robustos: si el usuario
   introduce de manera manual un URI para ver (Show) una película que no existe (por ejemplo /movies/99999), verás un mensaje de excepción
   horrible. Modifica el método show del controlador para que, si se pide una película que no existe, el usuario sea redirigido a la vista
   Index con un mensaje más amigable explicando que no existe ninguna película con ese.

Para ello, necesitamos modificar en la accion show del controlador para que, si se pide una película que no existe, el usuario sea redirigido a la vista Index con un mensaje.De tal manera que si el usuario introduce de manera manual un URI para ver (Show) una película que no existe controlemos dicha excepcion con `rescue ActiveRecord::RecordNotFound` 

   ```ruby
    def show
     id = params[:id]
     begin
     @movie = Movie.find(id)
     rescue ActiveRecord::RecordNotFound
     flash[:notice] = "No existe ninguna película con ese nombre."
     redirect_to movies_path
     return
     end
   end
```

Con la linea flash[:notice] = "No existe ninguna película con ese nombre." nos mostrara el mensaje en la vista index
debido a la siguiente linea redirect_to movies_path.


![Captura de pantalla de 2023-12-06 08-03-02](https://github.com/miguelvega/PracticaCalificada5/assets/124398378/3b2124a1-010d-4762-b7f9-ea8ea88ffd8d)

2. El método auth_hash  tiene la sencilla tarea de devolver lo que devuelva OmniAuth como resultado de intentar autenticar
   a un usuario. ¿Por qué piensa que se colocó esta funcionalidad  en su propio método en vez de simplemente referenciar     request.env[’omniauth.auth’]  directamente? Muestra el uso del script.

```
class SessionsController < ApplicationController
  def create
    @user = User.find_or_create_from_auth_hash(auth_hash)
    self.current_user = @user
    redirect_to '/'
  end

  protected

  def auth_hash
    request.env['omniauth.auth']
  end
end

```

Debido a que se recomienda tener la obtencion de autenticacion separado de un metodo para que de esta manera al escribir 
pruebas unitarias no se tenga que repetir el entorno completo de request.env en nuestras pruebas.


3. En las actividades relacionados a JavaScript, Siguiendo la estrategia del ejemplo de jQuery utiliza JavaScript para implementar un conjunto de casillas de verificación (checkboxes) para la página que muestra la lista de películas, una por cada calificación (G, PG, etcétera), que permitan que las películas correspondientes permanezcan en la lista cuando están marcadas. Cuando se carga la página por primera vez, deben estar marcadas todas; desmarcar alguna de ellas debe esconder las películas con la clasificación a la que haga referencia la casilla desactivada.

Para lograr la funcionalidad de mostrar solo las peliculas que marcamos en el checkboces debemos modificar la siguiente linea dentro de
archivo index.html.erb

```
 <%= check_box_tag "ratings[#{rating}]", '2', (@ratings_to_show_hash.include?(rating)),

```
Teniendo el siguiente resultado.

![Captura de pantalla de 2023-12-06 09-18-35](https://github.com/miguelvega/PracticaCalificada5/assets/124398378/e865a3fe-cb20-468e-95c8-fa1be62ca5cf)

