---
title: Callback Functions
localeTitle: Функции обратного вызова
---
В этой статье дается краткое введение в концепцию и использование функций обратного вызова на языке программирования Javascript.

## Функции - это объекты

Первое, что нам нужно знать, это то, что в Javascript функции являются первоклассными объектами. Таким образом, мы можем работать с ними так же, как мы работаем с другими объектами, например, назначая их переменным и передавая их в качестве аргументов в другие функции. Это важно, потому что это последний метод, который позволяет нам расширять функциональность в наших приложениях.

## Функции обратного вызова

Функция **обратного вызова** - это функция, которая передается _как аргумент_ другой функции, которая будет «возвращена» позже. Функция , которая принимает другие функции в качестве аргументов называется **функция высшего порядка,** которая содержит логику , _когда_ функция обратного вызова запускается на выполнение. Это сочетание этих двух, которые позволяют нам расширить нашу функциональность.

Чтобы проиллюстрировать обратные вызовы, давайте начнем с простого примера:

```javascript
function createQuote(quote, callback){ 
  var myQuote = "Like I always say, " + quote; 
  callback(myQuote); // 2 
 } 
 
 function logQuote(quote){ 
  console.log(quote); 
 } 
 
 createQuote("eat your vegetables!", logQuote); // 1 
 
 // Result in console: 
 // Like I always say, eat your vegetables! 
```

В приведенном выше примере `createQuote` - это функция более высокого порядка, которая принимает два аргумента, вторая - обратный вызов. Функция `logQuote` используется для передачи в качестве нашей функции обратного вызова. Когда мы выполняем функцию `createQuote` _(1)_ , обратите внимание, что мы _не_ `logQuote` круглые скобки к `logQuote` когда передаем его в качестве аргумента. Это связано с тем, что мы не хотим сразу же выполнить функцию обратного вызова, мы просто хотим передать определение функции вместе с функцией более высокого порядка, чтобы она могла быть выполнена позже.

Кроме того, нам нужно убедиться, что если функция обратного вызова, которую мы передаем, ожидает аргументы, мы предоставляем эти аргументы при выполнении обратного вызова _(2)_ . В приведенном выше примере это будет `callback(myQuote);` , так как мы знаем, что `logQuote` ожидает, что котировка будет передана.

Кроме того, мы можем передавать анонимные функции как обратные вызовы. Следующий вызов `createQuote` будет иметь тот же результат, что и вышеприведенный пример:

```javascript
createQuote("eat your vegetables!", function(quote){ 
  console.log(quote); 
 }); 
```

Кстати, вам не _нужно_ использовать слово «обратный вызов» в качестве имени вашего аргумента, Javascript просто должен знать, что это правильное имя аргумента. Исходя из приведенного выше примера, приведенная ниже функция будет вести себя точно так же.

```javascript
function createQuote(quote, functionToCall) { 
  var myQuote = "Like I always say, " + quote; 
  functionToCall(myQuote); 
 } 
```

## Зачем использовать обратные вызовы?

Большую часть времени мы создаем программы и приложения, которые работают **синхронно** . Другими словами, некоторые из наших операций запускаются только после завершения предыдущих. Часто, когда мы запрашиваем данные из других источников, таких как внешний API, мы не всегда знаем, _когда_ будут возвращены наши данные. В этих случаях мы хотим дождаться ответа, но мы не всегда хотим, чтобы наше приложение полностью остановилось, пока наши данные извлекаются. В таких ситуациях функции обратного вызова пригождаются.

Давайте рассмотрим пример, который имитирует запрос на сервер:

```javascript
function serverRequest(query, callback){ 
  setTimeout(function(){ 
    var response = query + "full!"; 
    callback(response); 
  },5000); 
 } 
 
 function getResults(results){ 
  console.log("Response from the server: " + results); 
 } 
 
 serverRequest("The glass is half ", getResults); 
 
 // Result in console after 5 second delay: 
 // Response from the server: The glass is half full! 
```

В приведенном выше примере мы делаем mock-запрос на сервер. По истечении 5 секунд ответ будет изменен, а затем будет выполнена функция обратного вызова `getResults` . Чтобы увидеть это в действии, вы можете скопировать / вставить вышеуказанный код в инструмент разработчика вашего браузера и выполнить его.

Кроме того, если вы уже знакомы с `setTimeout` , то вы все время используете функции обратного вызова. Анонимный аргумент функции, переданный в приведенный выше пример вызова функции `setTimeout` , также является обратным вызовом! Таким образом, исходный обратный вызов примера фактически выполняется другим обратным вызовом. Будьте осторожны, чтобы не вставлять слишком много обратных вызовов, если вы можете помочь, так как это может привести к чему-то, называемому «callback hell»! Как следует из названия, это не радость.