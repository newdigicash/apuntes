# Comandos de React Native
Listado de comandos más comunes de React Native

## 1. Metadatos

### Autor
@newdigicash

### Versión
0.1

## 2. Observación
Hay que tener en cuenta que algunas librerias como *request@2.88.2* están descontinuadas 
y son necesarias al instalar _explo-cli_, por eso aparecen advertencias.

## 3. Contenido 

### 3.1 Pre requisitos
Instalar una versión de [Nodejs LTS][urlNodejs].

Para crear un proyecto nuevo, hay 2 formas:

#### 3.1.1 Expo Cli

Expo facilita la creación del proyecto pero no da control para configurar y compilar la app.

Hay que instalar _explo-cli_ globalmente:
~~~
npm install -g expo-cli
~~~

#### 3.1.2 React Native Cli
React native cli esta hecho por el team de facebook y da control total al proyecto. 
La [documentación oficial][urlSetup] indica que *__no__ hay que instalar react-native-cli globalmente*, 
sino usar __npx__ que ya viene con _npm_.

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


## 4. Fuentes
Web oficial de RN <https://reactnative.dev>

Documentación oficial de RN <https://reactnative.dev/docs/getting-started>

Expo Cli <https://expo.io/tools>

Node <https://nodejs.org/>

[//]: # (referencias citadas)
[urlSetup]: https://reactnative.dev/docs/environment-setup
[urlNodejs]: https://nodejs.org/en/
