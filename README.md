// ==UserScript==
// @name         Google Homework_14
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       Samir Al-Kavvas
// @match        https://www.google.com/*
// @match        https://vc.ru/templatemonster/102062-top-10-luchshih-prilozheniy-app-store-i-google-play
// @icon         https://www.google.com/s2/favicons?sz=64&domain=google.com
// @grant        none
// ==/UserScript==
let keywords = ["ТОП-10 лучших приложений Google Play"];
let keyword = keywords[getRandom(0, keywords.length)];
let btnK = document.getElementsByName("btnK")[0];
let links = document.links;

if (btnK !== undefined){
    document.getElementsByName("q")[0].value = keyword;
    btnK.click();
} else {
    for (let i = 0; i < links.length; i++) {
    if (links[i].href.indexOf("vc.ru") !== -1){
        let link = links[i];
        console.log("Нашел строку " + link);
        link.click();
        break;
        }
    }
}
function getRandom(min, max) {
    return Math.floor(Math.random()*(max - min) + min);
}
