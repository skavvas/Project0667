// ==UserScript==
// @name         Google Bot DZ 15
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  try to take over the world!
// @author       Samir Al-Kavvas
// @match        https://www.google.com/*
// @match        https://vc.ru/templatemonster/102062-top-10-luchshih-prilozheniy-app-store-i-google-play*
// @icon         data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw==
// @grant        none
// ==/UserScript==

let sites = {
"vc.ru":["ТОП-10 лучших приложений App Store и Google Play", "Существует десяток подборок с самыми популярными приложениями App Store и Google Play"],
};

let keywords = ["ТОП-10 лучших приложений App Store и Google Play"];
let keyword = keywords[getRandom(0, keywords.length)];
let btnK = document.getElementsByName("btnK")[0]; //Кнопка поиска
let links = document.links; // Собираем все ссылки на странице
let googleInput = document.getElementsByName('q')[0]; //Поле ввода запроса
//let pnnext = document.getDocumentById('pnnext')


if (btnK !== undefined) { // Если мы на главной
  let i = 0;
  let timerId = setInterval(()=> { //Печатаем запрос
    googleInput.value += keyword[i]; //по одной букве
    i++;
    if(i == keyword.length) {
      clearInterval(timerId);
      setTimeout(()=>{
        btnK.click(); //кликаем по кнопке
      }, getRandom(200, 500));
    }
  }, 300);

// 2 часть скрипта,
} else if (location.hostname == 'vc.ru') { //проверяем на целевом ли мы сайте
setInterval(()=>{
let index = getRandom(0, links.length);
if (getRandom(0, 101) >= 80) {
location.href = 'https://www.google.com/';
}
if (links.length == 0) {
location.href = 'vc.ru';
}
if (links[index].href.indexOf("vc.ru") !== -1) {
links[index].click();
}
}, getRandom(3000, 5000));
console.log('мы на целевом сайте');

// 3 часть скрипта

} else { //если мы не нацелом сайте, а на страницах поисковой выдачи
  let nextGooglePage = true; //флаг говорит, надо ли идти дальше в поисковой выдаче
  for (let i = 0; i < links.length; i++) { //перебираем ссылки в цикле
    if (links[i].href.indexOf("vc.ru") !== -1) {
      let link = links[i];
      nextGooglePage = false;
      console.log("Нашел строку " + link);
      setTimeout(()=>{
        link.click();
      }, getRandom(1500, 4000));
      break;
    }
  } //конец цикла, где ищем целевой сайт в выдаче
  if (document.querySelector('.YyVfkd').innerText == '5') { //если не нашли в выдаче целевой сайт, ищем до 5 страницы
  nextGooglePage = false;
  location.href = 'https://www.google.com/';
  }
  if (nextGooglePage) { //кликаем по каждой странице выдачи
    setTimeout(()=>{
      pnnext.click();
    }, getRandom(2000, 5000));
  }
}

function getCookie(name) {
let matches = document.cookie.match(new RegExp(
"(?:^|; )" + name.replace(/([\.$?*|{}\(\)\[\]\\\/\+^])/g, '\\$1') + "=([^;]*)"
));
return matches ? decodeURIComponent(matches[1]) : undefined;
}

function getRandom(min, max) {
  return Math.floor(Math.random()*(max - min) + min);
}
