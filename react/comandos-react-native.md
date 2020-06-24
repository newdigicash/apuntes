# Comandos de React Native
Listado de comandos más comunes de React Native y librerias para el proyecto.

## 1. Metadatos

### Autor
@newdigicash

### Versión
0.3

## 2. Observaciones
+ Hay que tener en cuenta que algunas librerias como *request@2.88.2* están descontinuadas 
y son necesarias al instalar _explo-cli_, por eso aparecen advertencias.
+ Es mejor crear un proyecto con *React Native Cli*, desde el principio, porque varias 
librerías y componentes ya no son compatibles (o presentan bugs) con _Expo_. 

## 3. Contenido 

### 3.1 Pre requisitos
Instalar una versión de [Nodejs LTS][urlNodejs].

Para crear un proyecto nuevo, hay 2 formas:

#### 3.1.1 Expo Cli

Expo facilita la creación del proyecto pero no da control para configurar y compilar la app.

Hay que instalar _explo-cli_ globalmente antes de crear el proyecto:
~~~
npm install -g expo-cli
~~~

#### 3.1.2 React Native Cli
React native cli está hecho por el team de facebook y da control total al proyecto. 
La [documentación oficial][urlSetup] indica que *__no__ hay que instalar react-native-cli globalmente*, 
sino usar __npx__ que ya viene con _npm_.

Instalar [Android Studio y el SDK][urlAndroidStudio] tal como indica la [documentación oficial][urlSetup]. 
Luego configurar el emulador y verificar que este funcionando con *adb devices*.

Después de crear un proyecto, hay que revisar y configurar el _targetsdk_, _minsdk_, 
_compilesdk_, y _builtools_ en *android/app/build.gradle* antes de compilar.

### 3.2 Comandos de Expo
Cmd | Descripción
:-- | :--
`expo init nombre-proyecto` | crea un proyecto nuevo
`npm start` | compila y ejecuta en nagevador
`expo start` | compila y ejecuta en navegador

### 3.3 Comandos de React Native Cli
Cmd | Descripción
:-- | :--
`npx react-native init nombre-proyecto` | crea un proyecto nuevo
`npx react-native run-android` | compila y ejecuta en android 5+

### 3.4 Utilitarios para el proyecto

#### 3.4.1 Navegación entre pantallas (routing)
La [librería *React Navigation*][urlNavigation] permite la navegación básica en RN. 
En la [documentación][urlNavigation] está los comandos para la instalación y un ejemplo simple.

En la [documentación completa][urlNavigationFull] están las indicaciones para pasar parámetros en 
la navegación, configurar la cabecera, navegación con tabs y drawers, etc.

#### 3.4.2 Estilos y Maquetación
RN acepta [estilo inline][urlEstilo], ejemplo `style = {{color:'red'}}` y [estilo][urlEstilo] 
con *StyleSheet.create*. Las propiedades de los estilos son similares a CSS3, aunque usando camel case.

RN usa [Flexbox para construir el layout][urlRNFlex] y alinear los elementos. También hay 
[palabras clave de colores][urlColorKey] con sus equivalentes en RGB.

#### 3.4.3 Iconos

[FontAwesome][urlFontAwesome] tiene una [galería de iconos][urlFontAwesomeGallery]. 
En el [repo oficial][urlFontAwesome] están las indicaciones para instalar y usar.

#### 3.4.4 Visor de Markdown Nativo
Hay un [componente para renderizar Markdown][urlMarkdownRepo] en RN sin *Web View*. 
En el [repo oficial][urlMarkdownRepo] hay instrucciones para instalar y usar.

#### 3.4.5 Base de datos
[Realm Database][urlRealmDB] guarda los datos localmente (offline), es open source y 
ofrece soporte para varios lenguajes. 
En la [documentación oficial][urlRealmReact] hay indicaciones para instalar y un 
[tutorial][urlRealmReact] para empezar a usar en Javascript / RN. 
Además, la [documentación][urlRealmReact] explica con detalle la configuración, 
los modelos, consultas, etc. 

Aquí hay un [tutorial][urlReamlTuto] para implementar un [CRUD con Realm][urlReamlTuto] y RN.

#### 3.4.6 Acceso a arhivos locales
La [librería *react-native-fs*][urlRNFileSys] tiene una API que permite crear y leer carpetas, 
crear, leer, escribir, copiar archivos en Android e iOS. Además permite cargar 
archivos locales a una carpeta remota y viceversa. El [repo oficial][urlRNFileSys] explica la 
instalación y las funciones de la API con sus parámetros.

## 4. Fuentes
Web oficial de RN <https://reactnative.dev>

Documentación oficial de RN <https://reactnative.dev/docs/getting-started>

React Navigation <https://reactnavigation.org/docs/getting-started>

Expo Cli <https://expo.io/tools>

Node <https://nodejs.org/>

Android Studio <https://developer.android.com/studio>

[//]: # (referencias citadas)
[urlSetup]: https://reactnative.dev/docs/environment-setup
[urlNodejs]: https://nodejs.org/en/
[urlAndroidStudio]: https://developer.android.com/studio
[urlNavigation]: https://reactnative.dev/docs/navigation
[urlNavigationFull]: https://reactnavigation.org/docs/getting-started
[urlEstilo]: https://reactnative.dev/docs/style
[urlRNFlex]: https://reactnative.dev/docs/flexbox
[urlColorKey]: https://reactnative.dev/docs/colors
[urlFontAwesome]: https://github.com/FortAwesome/react-native-fontawesome
[urlFontAwesomeGallery]: https://fontawesome.com/icons?d=gallery
[urlMarkdownRepo]: https://github.com/iamacup/react-native-markdown-display
[urlRealmDB]: https://www.mongodb.com/realm/mobile/database
[urlRealmReact]: https://realm.io/docs/javascript/latest
[urlReamlTuto]: https://aboutreact.com/example-of-realm-database-in-react-native
[urlRNFileSys]: https://github.com/itinance/react-native-fs
