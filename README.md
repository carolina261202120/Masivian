# Masivian


Arbol Binario
Para Consumir la API 1. Crear un arbol binario. ingrese a la URL http://localhost:55790/api/Values
Para Consumir la API 2. Dado un arbol binario y dos nodos retornar el ancestro común más cercano.. ingrese a la URL
http://localhost:55790/api/Values?nodo1=44&nodo2=29


La aplicacion se construyo en ,net en visual studio 2012

Debe descargar el archivo .rar y abrir la aplicacion
Compilarla.

El desarrollo:

a) Consta de la clase: ArbolModel, donde se construyo el modelo del arbol

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace MvcApplication4.Controllers
{
    public class ArbolModel
    {
        public int Contenido { get; set; }
        public int Nivel { get; set; }
        public string raiz { get; set; }
        public int Nodo { get; set; }
        public int HijoDerecha { get; set; }
        public int HijoIzquierda{ get; set; }
        public int Padre { get; set; }
    }
}

b) Tiene un controlador donde se programaro las apis rest

using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Web.Http;

namespace MvcApplication4.Controllers
{
    public class ValuesController : ApiController
    {
        
         // GET api/values
        public IEnumerable<ArbolModel> Get()
        {
            return new ArbolModel[]
            {
                new ArbolModel()
                {
                    Contenido= 67,
                    Nivel = 1,
                    raiz = "S",
                    Nodo = 1,
                    HijoDerecha=76,
                    HijoIzquierda= 39,
                    Padre= 0,
                },
                new ArbolModel()
                {
                    Contenido= 39,
                    Nivel = 2,
                    raiz = "N",
                    Nodo = 1,
                    HijoDerecha=44,
                    HijoIzquierda= 28,
                    Padre= 67,
                },
                new ArbolModel()
                {
                    Contenido= 76,
                    Nivel = 2,
                    raiz = "N",
                     Nodo = 1,
                     HijoDerecha=85,
                    HijoIzquierda= 74,
                    Padre= 67
                },
                new ArbolModel()
                {
                    Contenido= 28,
                    Nivel = 3,
                    raiz = "N",
                     Nodo = 1,
                     HijoDerecha=29,
                    HijoIzquierda= 0,
                    Padre= 39
                },
                new ArbolModel()
                {
                    Contenido= 44,
                    Nivel = 3,
                    raiz = "N",
                     Nodo = 1,
                     HijoDerecha=0,
                    HijoIzquierda= 0,
                    Padre= 39
                }
                ,
                new ArbolModel()
                {
                    Contenido= 74,
                    Nivel = 3,
                    raiz = "N",
                     Nodo = 2,
                     HijoDerecha=0,
                    HijoIzquierda= 0,
                    Padre= 76
                }
                ,
                new ArbolModel()
                {
                    Contenido= 85,
                    Nivel = 3,
                    raiz = "N",
                     Nodo = 2,
                     HijoDerecha=87,
                    HijoIzquierda= 83,
                    Padre= 76
                }
                ,
                new ArbolModel()
                {
                    Contenido= 29,
                    Nivel = 4,
                    raiz = "N",
                     Nodo = 1,
                     HijoDerecha=0,
                    HijoIzquierda= 0,
                    Padre= 28
                }
                ,
                new ArbolModel()
                {
                    Contenido= 83,
                    Nivel = 4,
                    raiz = "N",
                     Nodo = 4,
                     HijoDerecha=0,
                    HijoIzquierda= 0,
                    Padre= 85
                }
                ,
                new ArbolModel()
                {
                    Contenido= 87,
                    Nivel = 4,
                    raiz = "N",
                     Nodo = 4,
                     HijoDerecha=0,
                    HijoIzquierda= 0,
                    Padre= 85
                }

            };

        }

        public int padre(int nodo1, int nodo2)
        {

            var lista = Get();
            //Consultamos el nivel de nodo1
            var qryNvlNodo1 =
                 from arb1 in lista
                 where arb1.Contenido.Equals(nodo1)
                 select arb1.Nivel;

            //Consultamos el nivel de nodo2
            var qryNvlNodo2 =
               from arb2 in lista
               where arb2.Contenido.Equals(nodo2)
               select arb2.Nivel;

            int NivelNodo1 = qryNvlNodo1.Last();
            int NivelNodo2 = qryNvlNodo2.Last();
            //Validamos cual es el nivel minimo, empezando de arriba N1 abajo N4
            int nivelMin = Math.Min(NivelNodo1, NivelNodo2);

            //Consultamos el contenido del nivel minimo
            var qryCntNodoNivMin =
                 from arb3 in lista
                 where arb3.Nivel.Equals(nivelMin) & (arb3.Contenido.Equals(nodo1) || arb3.Contenido.Equals(nodo1))
                 select arb3.Contenido;

            //Consultamos el padre del contenido del nivel minimo
            int ContenidoNivelMinimo = qryCntNodoNivMin.First();
            var qryPadreNivMin =
                 from arb4 in lista
                 where arb4.Contenido.Equals(ContenidoNivelMinimo)
                 select arb4.Padre;

            //Consultamos el nivel maximo
            int nivelMax = Math.Max(NivelNodo1, NivelNodo2);

            //Consultamos el contenido del nivel maximo
            var qryPadreNivMax =
             from arb5 in lista
             where arb5.Nivel.Equals(nivelMax) & (arb5.Contenido.Equals(nodo1) || arb5.Contenido.Equals(nodo2))
             select arb5.Contenido;

            int ContenidoNivelMAx = qryPadreNivMax.Last();

            //Consultamos el padre del contenido del nivel maximo
            var queryNivelMax =
             from arb6 in lista
             where arb6.Contenido.Equals(ContenidoNivelMAx)
             select arb6.Padre;

            int padreNivelMax = queryNivelMax.Last();
            int padreNivelMnimmo = qryPadreNivMin.Last();
            int iguales = 0;
            //Comparamos si los niveles de los padres son iguales 
            if (padreNivelMax == padreNivelMnimmo)
                //Si son iguales el padre es el de nivel maximo
            { iguales = padreNivelMax; }
            else
                //Si no son iguales llamamos el metodo recursivamente
            { iguales = padre(padreNivelMax, ContenidoNivelMinimo);}
            return iguales;

       }

        //API para conultr el padre ancestro
        public IEnumerable<ArbolModel> Get(int nodo1, int nodo2)
        {
            //obtenemos el padre ancestro de dos nodos dados ediante la funcion recursiva
            int padreancestro= padre(nodo1, nodo2);
            return new ArbolModel[]
            {
                new ArbolModel()
                {
                    Contenido = nodo1,
                    Padre = padreancestro,
                    raiz= "N"
                },
                new ArbolModel()
                {
                    Contenido = nodo2,
                    Padre = padreancestro,
                      raiz= "N"
                }
            };
         }
    }
}
   

c) Tiene una visa con las instrucciones para ver los 2 puntos de las apis

<div>

    <div>

        <h1>Arbol Binario</h1>       

       <h2>Para Consumir la API 1. Crear un arbol binario. ingrese a la URL  http://localhost:55790/api/Values </h2>  

        <h2>Para Consumir la API 2. Dado un arbol binario y dos nodos retornar el ancestro común más cercano.. ingrese a la URL <br /> http://localhost:55790/api/Values?nodo1=44&nodo2=29 </h2> 

    </div>


</div>

Cordialmente 

Carolina Olarte
