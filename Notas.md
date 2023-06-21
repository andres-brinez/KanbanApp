## Paso 1: Configuración del proyecto

Primero, crea un nuevo directorio para nuestro proyecto React, Abra su terminal incorporado y ejecute los siguientes comandos línea por línea:

- npx create-react-app kanban-app - se crea una aplicación React llamada kanban-app, Al ejecutar este comando, se crea una estructura de proyecto básica que incluye todas las dependencias necesarias para comenzar a desarrollar una aplicación de React. También se generan algunos archivos y carpetas preconfigurados para que pueda comenzar a codificar de inmediato.

- cd kanban-app - cambia el directorio a la carpeta kanban-app creada por React

- npm install react-draggable - instala el paquete react-draggable, Este paquete nos permite hacer que cualquier elemento se pueda arrastrar. Los usuarios podrán arrastrar los componentes a través del navegador web

- npm run start - inicia el servidor de desarrollo local, Este comando abre una nueva pestaña en su navegador web predeterminado. En esta pestaña, podrá ver los cambios que ha realizado en su aplicación React en vivo.

Ahora, creemos algunas carpetas nuevas para organizar los archivos con los que trabajaremos. En primer lugar, en la carpeta src , cree una carpeta llamada components .

## Paso 2: crear el encabezado de la aplicación web

Ahora se hace un componente de encabezado simple. Dentro de la carpeta de componentes , crea un archivo llamado Header.js . Para este encabezado, dibujaremos 2 elementos de texto dentro de nuestro elemento <header>.

En la línea 1, puede notar que estamos exportando el componente funcional, Header . La palabra clave export nos permite usar este componente en otros archivos JavaScript que hayamos creado en el proyecto React. Importante

    export function Header () {
        return(

            <header style={styles.header}>
            <h1 style={styles.title}>Kanban Board</h1>
            <p style={styles.subtitle}>Built by stackup-username</p>
            </header>
        )
    }

- Despues en una constante se agregan los estilos

  const styles={ estilos}

## Paso 3: Importar estados de cuenta (Statements)

En el archivo App.js escribiremos la mayor parte de la lógica de la aplicación web Kanban en este archivo.

En primer lugar, reemplace el código existente en App.js con el siguiente código. Tendremos que importar varios paquetes y las dependencias arrastrables que descargamos anteriormente en el paso 1.

- import React, { useState, useEffect } from "react"; - importa el paquete React y los hooks useState y useEffect que sirve para crear estados y efectos

hook nos permite crear un estado que se puede cambiar. Un Estado es simplemente una variable que React monitoreará internamente en segundo plano. Cuando cambia el estado, cambiará el contenido que se muestra en la pantalla en lugar de volver a mostrar toda la página web. Este comportamiento hará que una aplicación React sea eficiente.

- import { Header } from "./components/Header"; - importa el componente Header que creamos en el paso 2
- import Draggable from 'react-draggable'; - importa el paquete react-draggable que descargamos en el paso 1

## Paso 4: Estructura App.js

Crearemos la estructura para la aplicación principal.

    export default function App() {

    const [board, setBoard] = useState([]) - crea un estado llamado board que es un array vacio, y el setBoard es una funcion que se encarga de actualizar el estado board

    return (
        <div>

        <Header />

            <div style={styles.boardContainer}>

                {board.map((list) => { - recorre el array board

                    return (
                        - retorna un div con la informacion de la lista

                        <div id={`list_${list.id}`} key={list.id} className="list-container" style={styles.listContainer}>

                        <h2>{list.title}</h2>

                        - crea un boton para agregar una nueva tarjeta
                        <button

                            style={styles.newCard} - estilos del boton

                            onClick={() => { - al hacer click se agrega una nueva tarjeta

                                let temp_boards = [...board] - crea una copia del array board

                                - recorre el array board
                                for (let i = 0; i < temp_boards.length; i++) {

                                    - si el id de la lista es igual al id de la lista del array board
                                    if (temp_boards[i].id === list.id) {

                                        - se agrega una nueva tarjeta al array board copia
                                        temp_boards[i].cards.push({

                                            id: new Date().getTime(),
                                            title: 'New Card',
                                            description: 'New Card Description'

                                        })
                                    }
                                }

                                setBoard(temp_boards) - se actualiza el estado board

                            }} >+ New Card</button>


                        {list.cards.map((card) => { - recorre el array cards
                            return (

                                <Draggable - se agrega el paquete react-draggable

                                    key={card.id} -
                                    onStop={(e,) => { -

                                    }}
                                    >

                                    <div style={styles.cardContainer}>
                                        <input type={"text"} style={styles.title} value={card.title}
                                            onChange={(e) => {

                                            }}
                                        />
                                        <input type={"text"} style={styles.description} value={card.description}
                                            onChange={(e) => {

                                            }}
                                        />
                                    </div>

                                </Draggable>
                            )
                        })}
                            </div>
                        )
                })}
            </div>
        </div>
    );
    }

## Paso 5: Lógica de la aplicación

El código anterior solo creará nuevas tareas. Tendremos que agregar algo de lógica para que podamos mover la tarea alrededor de cada cubo. Básicamente, nos gustaría determinar si el usuario ha liberado la tarea dentro del depósito. Una vez que hayamos determinado eso, necesitaremos obtener el depósito de origen y el depósito de destino. La idea detrás de esto es clonar la tarea desde el depósito de origen al depósito de destino. Después de eso, eliminaremos la tarea del depósito de origen.

Este código solo se ejecutará cuando el usuario haya dejado de arrastrar el componente de la tarea. La idea del algoritmo es determinar la posición actual de la tarea y si la tarea se encuentra dentro del rectángulo del cubo. A veces, el usuario simplemente arrastraba la tarea y no la arrastraba hacia el cubo. Nosotros podemos usar.

El codigo va dentro de la función onStop del componente Draggable que se encuentra en el archivo App.js 

    let allLists = document.querySelectorAll('.list-container'); - para obtener todos los componentes <div> que tienes la clase list-container

    for (let i = 0; i < allLists.length; i++) { - recorre el array allLists que contiene el tablero para determinar en qué cubo se encuentra. Si la tarea se encuentra dentro del cubo, podemos agregar la tarea al cubo.

            let list = allLists[i]; 
            let rect = list.getBoundingClientRect(); - Para determinar sobre qué depósito se arrastra la tarea, podemos usar getBoundingClientRect()
            let data = { - crea un objeto data que contiene la posicion del mouse

            x: e.clientX,
            y: e.clientY
            
            }

            let flag = false - 
            
            if (data.x > rect.left && data.x < rect.right && data.y > rect.top && data.y < rect.bottom) { - si la posicion del mouse esta dentro del rectangulo del cubo se ejecuta el codigo

            let final*list_id = list.id.split('*')[1]; - obtiene el id del cubo
            let final_card_id = card.id; - obtiene el id de la tarea
            let temp_boards = [...board] - crea una copia del array board

           
            for (let boardIndex = 0; boardIndex < temp_boards.length; boardIndex++) { - 
                for (let cardIndex = 0; cardIndex < temp_boards[boardIndex].cards.length; cardIndex++) {
                    if (temp_boards[boardIndex].cards[cardIndex].id === final_card_id) {
                        temp_boards[boardIndex].cards.splice(cardIndex, 1) - elimina la tarea del array board 
                    }
                }
            }
            for (let boardIndex = 0; boardIndex < temp_boards.length; boardIndex++) {
                if (temp_boards[boardIndex].id === parseInt(final_list_id)) {- si el id del cubo es igual al id del cubo del array board copia
                    temp_boards[boardIndex].cards.push(card) - se agrega la tarea al array board copia
                }
            }
            flag = true
            setBoard(temp_boards)- se actualiza el estado board
            }            

    }

- Dentro de la funcion onChange se agrega el siguiente codigo 

    Las siguientes dos  logicas son similares solo que una actualiza el titulo y la otra la descripcion de la tarea


        let temp_boards = [...board]
            
            for (let i = 0; i < temp_boards.length; i++) {- Determinar el objeto exacto en el que se encuentra cada tarea dentro del estado del tablero 
              for (let j = 0; j < temp_boards[i].cards.length; j++) {

               if (temp_boards[i].cards[j].id === card.id) {
                temp_boards[i].cards[j].title = e.target.value - actualiza el titulo de la tarea
               }

              }
             }
             setBoard(temp_boards)

        let temp_boards = [...board]
                for (let i = 0; i < temp_boards.length; i++) {
                    for (let j = 0; j < temp_boards[i].cards.length; j++) {
                        
                        if (temp_boards[i].cards[j].id === card.id) {
                            temp_boards[i].cards[j].description = e.target.value - actualiza la descripcion de la tarea
                     
                        }
                    }
                }
                setBoard(temp_boards)

## Paso 6: Diseño de Kanban

Ahora agregaremos el estilo al final del archivo App.js con una const styles={ estilos}

## Paso 7: use Effect