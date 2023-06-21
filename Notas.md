##  Paso 1: Configuración del proyecto

Primero, crea un nuevo directorio para nuestro proyecto React, Abra su terminal incorporado y ejecute los siguientes comandos línea por línea:

- npx create-react-app kanban-app - se crea una aplicación React llamada kanban-app, Al ejecutar este comando, se crea una estructura de proyecto básica que incluye todas las dependencias necesarias para comenzar a desarrollar una aplicación de React. También se generan algunos archivos y carpetas preconfigurados para que pueda comenzar a codificar de inmediato.

- cd kanban-app - cambia el directorio a la carpeta kanban-app creada por React

- npm install react-draggable - instala el paquete react-draggable, Este paquete nos permite hacer que cualquier   elemento se pueda arrastrar. Los usuarios podrán arrastrar los componentes a través del navegador web    

- npm run start - inicia el servidor de desarrollo local, Este comando abre una nueva pestaña en su navegador web predeterminado. En esta pestaña, podrá ver los cambios que ha realizado en su aplicación React en vivo.

Ahora, creemos algunas carpetas nuevas para organizar los archivos con los que trabajaremos. En primer lugar, en la carpeta src , cree una carpeta llamada components .

## Paso 2: crear el encabezado de la aplicación web
Ahora se hace  un componente de encabezado simple. Dentro de la carpeta de componentes , crea un archivo llamado Header.js . Para este encabezado, dibujaremos 2 elementos de texto dentro de nuestro elemento <header>. 

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


## Paso 3: Importar estados de cuenta
Abramos el archivo App.js ya que escribiremos la mayor parte de la lógica de la aplicación web Kanban en este archivo. En primer lugar, reemplace el código existente en App.js con el siguiente código. Tendremos que importar varios paquetes y las dependencias arrastrables que descargamos anteriormente en el paso 1.
 En la línea 1 del archivo actual, necesitaremos usar dos ganchos integrados en ReactJS. Estos dos ganchos son useState y useEffect . El estado de uso
import React, { useState, useEffect } from "react";
import { Header } from "./components/Header";
import Draggable from 'react-draggable';

hook nos permite crear un estado que se puede cambiar. Un Estado es simplemente una variable que React monitoreará internamente en segundo plano. Cuando cambia el estado, cambiará el contenido que se muestra en la pantalla en lugar de volver a mostrar toda la página web. Este comportamiento hará que una aplicación React sea eficiente.