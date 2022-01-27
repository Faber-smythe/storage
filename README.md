<h1 align="center">DVIC-Verse</h1>
<p align="center">
  An interactive 3D experience of the labs premises
</p>

<!-- TABLE OF CONTENTS -->
<details open="open">
  <summary style="display: flex; align-items: center"><h2>Table of Content</h2></summary>
  <ol>
    <li>
      <a href="#description">Description</a>
      <ol>
        <li><a href="#origin">Origin</a></li>
        <li><a href="#feature-changes">Feature Changes</a></li>
        <li><a href="#structure">Structure</a></li>
      </ol>
    </li>
    <li>
      <a href="#running-the-project">Running the project</a>
    </li>
    <li><a href="#content-management">Content Management</a>
      <ol>
        <!-- <li><a href="#wheres-what">Where's what</a></li> -->
        <li><a href="#editing-a-scene">Editing a scene</a></li>
        <li><a href="#creating-a-scene">Creating a new scene</a>
          <ul>
            <li><a href="#data">Data</a></li>
            <li><a href="#assets">Assets</a></li>
          </ul>
        </li>
      </ol>
    </li>
    <li>
      <a href="#testing-exercise">Testing Exercise</a>
    </li>
  </ol>
</details>

## Description

### <b id="origin">ORIGIN</b>

The De Vinci Innovation Center (DVIC) only site currently has a web page where users can have a look at 3D scenes of the facility. This web application has been reported as having several issues, which this projects aims at solving.

This project is the alternative meant to replace the currently available application for the DVIC virtual visit.
The new solution is building things differently, in order to achieve smoother controls, better performance, easier maintenance and expansion and better overall user experience.

### <b id="feature-changes">FEATURE CHANGES</b>

Any preexisting features have been implemented in the new visit. In addition, a few things have been added.

- The navigation on the side menu now dynamically displays which scene is currently active
- The logo in the side menu now links to the homepage of the DVIC website
- The current scene is now held within the browser url for
- The visit is now mobile-responsive
- The POI (points of interest) now have labels when the cursor is hovering on them
- An entire development mode is now available to allow live editing in a WYSIWYG-like way, through a query parameter in the url.

Please note that while every feature has been kept, some of them have changed a little :

- Controls are smoother (camera orientation, zooming)
- Performance has improved (shorter loading, better frame rate, no saturated memory)
- The content of the POI (panels, iframes) are now displayed in the side menu.
- The graphic material has changed (sprites, arrows, icon...)

### <b id="structure">STRUCTURE</b>

As of now, the project has neither a back-office nor a database. This means it primarily consists in a single nuxt repository holding the front-end application. It does however include edition features allowing live modification of the scene’s object, in order to adress the maintainability and content management issues. The main components are described in the flowchart beleow.

<!-- ![](https://raw.githubusercontent.com/Faber-smythe/storage/main/dvic-visit-flowchart.png) -->


## Running the project

To run the application localy, follow the steps below.

1. Clone the project
   ```sh
   git clone https://github.com/DeVinci-Innovation-Center/dvic-verse
   ```
2. Install NPM packages
   ```sh
   npm install
   ```
3. Run the app
   ```sh
   npm run dev
   ```
   The page should be available at `http://localhost:3000/`

## Content Management

### <b id="editing-a-scene">EDITING A SCENE</b>

In order to edit a scene, you have to open the visit in a browser, and you need access to the project files.

- Once you're in the scene you want to edit, append `&mode=dev` to the current url, and enter `vinci` when prompted for a password
- Now you can use right-click to careful drag objects around, like arrows, POIs or even the scenes textures. Any selection is also logged in the side menu for further modifications.
- When you're done, before you leave the scene, download the JSON file and put it in the `/data/scenes/` folder to replace the old one.

> <i>if you are not sure of your changes, refresh the page and changes will be discarded</i>

### <b id="creating-a-scene">CREATING A SCENE</b>
#### <b id="data">Data files</b>

In order to create a scene, you first have to create the data objects in the appropriate json files.

- Add an entry in the `/data/map.json` file. It has to respect the following fields :
```json
  "new-scene": {
    "file": "newscene.json",
    "name": "My New Scene",
    "isDefault": false,
    "enabled": true
  }
```
Note that the slug (the scene's name in the URL) will be "new-scene" in the example above.
- You also have to create the `/data/scenes/newscene.json` file. The data has to respect the fields described in `/types/SceneFile.ts` (you can start by copying an existing scene's file). For example :
```json
{
  "texture": "02_entree.jpg",
  "sceneAzimuth": 0.0236,
  "arrows": [
    {
      "arrowAzimuth": 3.1393,
      "targetScene": "elevators"
    },
    {
      "arrowAzimuth": 0.1695,
      "targetScene": "atrium"
    }
  ],
  "POI": []
}
```

#### <b id="assets">Assets files</b>
Now that the data exists in the configuration files, you have to make sure the texture file you want to use are located correctly. The `newscene.json` file has a string field called "texture". The value must be the name a jpg file of the equirectangular spheremap (should be 4K while under 5Mo). The same name must be used for a low resolution version of the same texture (should be 1024 while under 150Ko).
- The standard resolution texture must be located under `/assets/textures/spheremaps/my-texture.jpg
- The low resolution texture must be located under `/assets/textures/spheremaps/lowres/my-texture.jpg

## Editing Exercise

The scene called `Atrium` is all over the place. Using the explanation of the <a href="#editing-a-scene">scene editing section</a>, please fix the following issues :

- Replace "telling story" with "storytelling" in the panel
- Correct the typo in the staff member's caption
- One of the POI is a little off, put it in the right position

> <i>You can use the content of `/data/wrong-atrium.json` to restore the false data of the scene</i>
