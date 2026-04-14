Eres un experto en analisis, diseño y desarrollo de sistemas en Next 16 prisma ORM. Quiero que crees una carpeta donde documentes siempre todo lo que se va avanzando en el proyecto, los cambios principales y estructura principal de todo esto, ahora también quiero que hagas plan de desarrollo como en antigravity, tip usarias eso como artefacto y un "task" que vayas haciendo check para las tareas que quedarian pendiente de inciio a fin en la app
ya instale el npx shadcn@latest add sidebar-07  
para usar dashboard en mi proyecto, asi que quizas tenga que usar 2 layouts.. uno para el tema auth para registro y login (solo usare username y contraseña. sin email ni nada mas. tanto para registro y para login. para registro quizás solo pediría el nombre de la persona y ya).

luego otro layout para el tema de dashboard. eso quiero que lo esturcutres bien. para que este bien modularizado esto.. 

contexto de la app :
la finalidad es mandar datos a n8n para generar comentarios enbase a parametros.

n8n de por si debe recibir primero una orden de generacion. 

Esta orden de generacion deberia tener parametros como "link de la publicacion" la red social,  "el target" que solo el target es un nombre clave para que luego otra persoan cuando solicite por http filtre por target dependiendo del cliente, debe tener un estado de generacion, si ya fue la orden generada. si sigue en borrador o fue eliminado. luego tambien debe tener una "intencion de comentarios" ya que eso ayudaria al generador de comentarios tener como una direccion. eso debe ser solo un campo de entrada de texto libre y la cantidad de comentarios a generar, luego tambien si la publicacion es video, imagen o   solo texto.  (como solo estara ambientado para instagram, tiktok, facebook o youtube de momento, creo que solo en facebook se postea con solo texto). 


luego estos comentarios se almacenaran en una tabla desde n8n para bdd. 

y ya ahi es otro proceso. la tabla se llama "comentarios_bots" donde se alamcenan los ocmentarios. ahora quiero que estos comentarios se muestren en la app.. obviamente se mostraran tarjetas por cada "target" asi que seguramente tambien haya modulo "target" donde se cree solo su nombre y ya. por cada target se agruparia por ejemplo los comentarios.. en el frotnend, yo veria como que los targets. seelecciono uno y veria los comentarios generados para ese target. y luego la orden de generacion para ese target. 

no le pongas "target" ponle otra cosa, pero debe ser asi de intuitiva la aplicacion .. . con esos modulos. tambien el modulo de usuarios. crealo para ver desde un rol adminsitrador (si mejor si tiene sistema de roles) para que yo pueda ver por ejemplo la lista de usuarios y que targets creo cada usuario, cuantas y que orden de generacion de comentarios y cuantos comentarios ya genero... asi que ten en cuenta todo eso 