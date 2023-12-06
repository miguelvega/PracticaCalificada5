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

   Para ello, necesitamos modificar en la accion show del controlador para que, si se pide una película que no existe, el usuario sea redirigido a la vista
   Index con un mensaje.De tal manera que si el usuario introduce de manera manual un URI para ver (Show) una película que no existe controlemos
   dicha excepcion dentro de con `rescue ActiveRecord::RecordNotFound`.

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

Con la linea `flash[:notice] = "No existe ninguna película con ese nombre."` nos mostrara el mensaje en la vista index
gracias a  `redirect_to movies_path`.


![Captura de pantalla de 2023-12-06 08-03-02](https://github.com/miguelvega/PracticaCalificada5/assets/124398378/a0b5e1c1-dea5-40f0-965c-7c54fc20ca62)![Captura de pantalla de 2023-12-06 08-03-02](https://github.com/miguelvega/PracticaCalificada5/assets/124398378/39cc2eca-4752-4bcc-aabb-7a61f49bdf1b)![Captura de pantalla de 2023-12-06 08-03-02](https://github.com/miguelvega/PracticaCalificada5/assets/124398378/39cc2eca-4752-4bcc-aabb-7a61f49bdf1b)

