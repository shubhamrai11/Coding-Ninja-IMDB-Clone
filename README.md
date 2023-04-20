> # [IMDb|dꓷMI](https://github.com/shubhamrai11/ "Want to try? Hit this link")
![](media/1.png)
 >## Initialising

 1. As the page loads
  1. **initialising()** function gets called
  1. and result willl be like this
  
  
  ![](media/1.gif)

  ---

 >## Search Movie

1. To search type movie name(**related Keywords**) 
1. search result will **update** as you enter any Keywords(key) 


  ![](media/2.gif)
  ---
 >## Add to WatchList and Favorite_List
1. Below  every card, there are two buttons
1. By pressing any one of then, you can either add to **watchlist** or **favorites**


  ![](media/list.gif)
---
>## Movie Page
   ![](media/3.png)

1. If you want to see any movie details, Just tap on it's image ( home-screen / search-result / favorite or watchlist anywhere)
1. A link is attached with every image , that will take you to the movie page
---
> **JAVAscript**

```javascript
// these are some APIs which are required in this project
const API_KEY = "api_key=c267432625feb2465de6db04863675fe"
const api_popular_movies = `https://api.themoviedb.org/3/movie/popular?${API_KEY}`;
const api_top_rated_movies = `https://api.themoviedb.org/3/movie/top_rated?${API_KEY}`;
const api_popular_tv_shows = `https://api.themoviedb.org/3/tv/popular?${API_KEY}`
const api_top_rated_tv_shows = `https://api.themoviedb.org/3/tv/top_rated?${API_KEY}`;
const api_img = "https://image.tmdb.org/t/p/w500";
const api_search = `https://api.themoviedb.org/3/search/multi?${API_KEY}&query=`;

// these are some constants 
const popular_movies = document.querySelector("#popular_movies");
const top_rated_movies = document.querySelector("#top_rated_movies");
const popular_tv_Shows = document.querySelector("#popular_tv_Shows");
const top_rated_tv_Shows = document.querySelector("#top_rated_tv_Shows");
const Favorite_list = document.querySelector(".favlist");
const fav_btn = document.querySelector(".fav_btn");
const WatchList = document.querySelector(".watchlist");
const watch_btn = document.querySelector(".watch_btn");
const input = document.querySelector("#fetch_input");
const fill_the_page = document.querySelector(".full_datails");
const search_result = document.querySelector(".search_result");
```
 these are some APIs and some constants which are required in this project

---
> **showNotification(** massage **)**
```javascript
// to give a massage
function showNotification(massage) {
    document.querySelector(".massage").classList.toggle("off");
    document.querySelector(".massage").innerHTML = massage;
    console.log(massage);
    setTimeout(() => {
        document.querySelector(".massage").classList.toggle("off");
    }, 3000);
}
```
* This function is used to show a notification(massage).
* It's just changing innerHTML and toggling **off**(class) and 
* toggling off(class) again inside the setTimeout function

---
> **create_and_append(** category, title, id, date, path **)**

```javascript
// append in home and search_div 
function create_and_append(category, title, id, date, path) {
    if (path == null) return;

    var list = document.createElement('div');
    list.className = "poster";
    list.innerHTML = ` 
     <div class="image"><a href="Movie_Page.html?${id}" ><img data-id="${id}" src="${api_img + path}"></a></div>
     <div class="details"><p>${title}</p><p>${date}</p></div>
    <button class="btn_1" data-id="${id}"><i class="bi bi-plus-square" style="color: aqua;"></i>Watchlist</button>
    <button class="btn_2" data-id="${id}"><i class="bi bi-heart" style="color: red;"></i>Favorites</button>
         `;
    category.append(list);
}
```
*  This function Takes 5 Arguments
   1. **catogery** (like popular_movies, popular_tv_Shows, Search_result) in which , this function has to append
   1. **title** (movie title/name)
   1. **id** (movie id, which will be used as dataset-id)
   1. **date**  (movie release date)
   1. **path** (image src path)

*  then create an element using these information
*  and then append element to **category**.
---

> **create_and_append_list(** pass, element **)**

```javascript
// appeend in Favorite_list and WatchList 
async function create_and_append_list(pass, element) {
    var api_movie_id = `https://api.themoviedb.org/3/movie/${pass}?${API_KEY}`;
    var result = await fetch(api_movie_id).then(response => {

        if (response.status >= 200 && response.status <= 299) {
            return response.json();
        } else {
            console.log(response.statusText + ` in ${pass} id`)
        }
    })

    if (result == undefined) return;

    localStorage.setItem(`${pass}`, element === WatchList ? "W" : "F");
    var child = document.createElement('div');

    child.className = "list";
    child.innerHTML = `
                 <div class="list_image">
                 <a href="Movie_Page.html?${result.id}" ><img data-id="${result.id}" src="${api_img + result.poster_path}"></a></div>
                <div class="list_detail"><p>${result.title}</p><p>${result.release_date}</p></div>
                <button class="delete" data-id="${result.id}"></button><i class="bi bi-trash3" style="color: red;"></i>
                `;
    element.append(child);
}
```

*  This function is used to append element in **Favorite_list and WatchList**
* Two arguments are passed inside this function
  1. **pass**  (movie id)
  1. **element** (where to append)
* just fetching details using api( **pass** )
* adding key-value pair inside localStorage
* creating element with fatched details
* and then appending

---
> **fill_the_details(** pass **)**
```javascript
//  movie detail html page
async function fill_the_details(pass) {
    var api_movie_id = `https://api.themoviedb.org/3/movie/${pass}?${API_KEY}`;
    var result = await fetch(api_movie_id)
        .then(response => response.json())
        .catch(error => showNotification(error + " SORRY  :-("));

    var child = document.createElement("div");
    child.className = "full_details";
    // to rotate image if screen width is small
    var image_path = (screen.width < 500) ? result.backdrop_path : result.poster_path;
    child.innerHTML = `
            <div class="full_poster">
            <img id="image_full" src= "${api_img + image_path}">
            </div>
            <div class="details_movie">
             <p>Movie Title : <br> ${result.title}</p>
             <p>Release Date : <br>  ${result.release_date}</p>
             <p>Movie Overview : <br>  ${result.overview}</p>
             <p>Average Vote : <br>  ${result.vote_average} (${result.vote_count} Votes)</p>
            </div>
        `;
    document.querySelector(".container").append(child);
    console.log("Movie ID : " + queryString + " ,  Movie Name: " + result.title);
    return;
}
```
* This function is used to fill movie's full details on **Movie Page**
* fetching details using movie,s ID (**pass** which is the argument passed in this function)
* creating an element ,adding to innerHTML and then appending

---
> **fill_category(** api, category **)**
```javascript
//  fill category like top_movie tv etc
async function fill_category(api, category) {

    var result = await fetch(api).then(response => {  // try and catch handling
        if (response.status >= 200 && response.status <= 299) {
            return response.json();
        } else {
            throw Error(response.statusText);
        }
    }).then(data => data.results).catch(error => showNotification(error));

    if (result == undefined) {
        return;
    }

    if (category == popular_movies || category == top_rated_movies || category == search_result) {
        for (let i = 0; i < result.length; i++) {
            var element = result[i];
            create_and_append(category, element.title, element.id, element.release_date, element.poster_path);
        }
    } else if (category == popular_tv_Shows || category == top_rated_tv_Shows) {
        for (let i = 0; i < result.length; i++) {
            var element = result[i];
            create_and_append(category, element.name, element.id, element.first_air_date, element.poster_path);
        }
    }
}
```
* this function is called when initialising home page and when someone search anything
* Two arguments **API** and **category** are passed in this function
* fetching details and then running for-loop and calling **create_and_append()** function with information as arguments.

---

> **append_list(** pass, element **)**
```javascript
// add to Favorite_list or WatchList and in localStorage
async function append_list(pass, element) {
    var api_movie_id = `https://api.themoviedb.org/3/movie/${pass}?${API_KEY}`;
    var result = await fetch(api_movie_id).then(response => response.json()).success;

    if (result == false) {
        showNotification("Unable to fetch from API,ERROR SORRY!!");
        return;
    }
    if (localStorage.getItem(pass) != null) {  //  is item already exist in Favorite_list or WatchList?
        console.log(`Selected item is already exist in ${localStorage.getItem(pass) == 'W' ? "Watchlist" : "Favorite List"}. First, delete from there and then select again`);
        return;
    }
    create_and_append_list(pass, element);
}
```
* This function is used to add(or append) favorite and watchList movies inside favorite and watchlist div ,and
* add movie's id as key and ("W" for watchlist, "F" for favorite) value pair inside localStorage
* first it check, is the movie already present in favorite or watchlist?
* if not, only then it will call **create_and_append_list(** pass, element **)** function to append.

---
> **fill_list()**
```javascript
// initialising Favorite_list and WatchList
function fill_list() {
    for (let i = 0; i < localStorage.length; i++) {
        var pass = localStorage.getItem(localStorage.key(i));
        if (pass === 'W' || pass === 'F') { // same local storage is sharing data with alarm clock as well( to seperate them, this if condition)
            create_and_append_list(localStorage.key(i), pass === "W" ? WatchList : Favorite_list);
        }
    }
}
```
* this function will be called from **initialising()** function, when someone refresh or re-visit the page 
* a for_loop will run and then it call **create_and_append_list()** with required arguments (i.e. **movie id** and **where to append**)

---
> **handleClickListener(** event **)**
```javascript
// click keyboard event handling and delete button
function handleClickListener(event) {
    const target = event.target;
    if (target.classList.contains('btn_1')) {
        append_list(parseFloat(target.dataset.id), WatchList);
        return;

    } else if (target.classList.contains('btn_2')) {
        append_list(parseFloat(target.dataset.id), Favorite_list);
        return;
    } else if (target.classList.contains("delete")) {
        localStorage.removeItem(target.dataset.id);
        WatchList.innerHTML = Favorite_list.innerHTML = "";
        showNotification("Successfully Deleted!!");
        fill_list();
        return;
    }
}
```
* this function will be called when someone wants to add movies or wants to delete a movie from ( favorite or watchList )

---
> **searchFunction()**
```javascript
// search
function searchFunction() {
    if(search_result.classList.contains('off')) return;
    search_result.innerHTML = "";
    fill_category(api_search + input.value, search_result);
}
```
* this function will be called when someone wants to search any movie
* then it will call another function **fill_category(** api, category **)** with arguments as what some enter in input-form
* basically, it will be called when a keyup event happens

---
> **these are event-Listeners**
```javascript

input.addEventListener('click', () => {
    fav_btn.classList.remove("active_list");
    Favorite_list.classList.remove("on");
    watch_btn.classList.remove("active_list");
    WatchList.classList.remove("on");
    search_result.classList.toggle('off');
})

fav_btn.addEventListener('click', () => {
    fav_btn.classList.toggle("active_list");
    Favorite_list.classList.toggle("on");
    watch_btn.classList.remove("active_list");
    WatchList.classList.remove("on");
    search_result.classList.add('off');
    search_result.innerHTML = "";
})

watch_btn.addEventListener('click', () => {
    watch_btn.classList.toggle("active_list");
    WatchList.classList.toggle("on");
    fav_btn.classList.remove("active_list");
    Favorite_list.classList.remove("on");
    search_result.classList.add('off');
    search_result.innerHTML = "";
})
document.addEventListener('keyup', searchFunction);
document.addEventListener('click', handleClickListener);

```
---
> **initialising()**
```javascript
async function initialising() {
    await fill_category(api_popular_movies, popular_movies);
    await fill_category(api_top_rated_movies, top_rated_movies);
    await fill_category(api_popular_tv_shows, popular_tv_Shows);
    await fill_category(api_top_rated_tv_shows, top_rated_tv_Shows);
    await fill_list();
}
```
 * this function will be called once when someone visit/refersh the page 
 * and then call **fill_category()** and **fill_list**()
 to fill the page and favorite and watchlist.

---
```javascript

var queryString = location.search.substring(1);

if (queryString !== "") {  // means movie page
    fill_the_details(queryString);
    fill_list();
} else {
    initialising();  // home page
}
```
* here just if-else condition to check whether it is a Home_page or Movie_page.

