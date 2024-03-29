**колбэки**  
Колбэк это функция, которая выполнится после другой функции, завершившей своё выполнение. Следовательно, отсюда и название, ‘call back’.  
  
Передадим функцию callback вторым аргументом в loadScript, чтобы вызвать её, когда скрипт загрузится
```  
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;
  script.onload = () => callback(script);
  document.head.append(script);
}

loadScript('https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js', script => {
  alert(`Здорово, скрипт ${script.src} загрузился`);
  alert( _ ); // функция, объявленная в загруженном скрипте
});
```
Такое написание называют асинхронным программированием с использованием колбэков. В функции, которые выполняют какие-либо асинхронные операции, передаётся аргумент callback — функция, которая будет вызвана по завершению асинхронного действия.  
 
Мы поступили похожим образом в loadScript, но это, конечно, распространённый подход.  

**Промисы**  
Начинается процесс написания промиса с его создания. Осуществляется это с помощью конструктора, т.е. с new Promise()  
Функцию resolve() вызывают обычно в том месте кода, в котором асинхронная операция должна завершиться успешно. А функцию reject() – там, где она должна завершиться с ошибкой.  
Промис завершается после вызова resolve() или reject(). При этом его состояние переходит соответственно в выполнено (state: "fulfilled") или отклонено (state: "rejected").  
Внутрь функций resolve() или reject() можно поместить аргумент, который затем будет доступен соответственно в then() или catch().  
```  
const passexam = true;
// промис
const result = new Promise((resolve, reject) => {
  setTimeout(() => {
    passexam ? resolve('Папа подарил 100$.') : reject('Папа не подарил 100$.');
  }, 5000);
});

result
  .then(value => {
    console.log(result);
    console.log(value);
  })
  .catch(value => {
    console.log(result);
    console.error(value);
  });
```
Когда passexam равен true, промис успешно завершится через 5 секунд посредством вызова функции resolve(). А так как промис завершился успешно, то будет вызван метод then()  
Если значение константы passexam поменять на false, то промис завершится с ошибкой через 5 секунд с помощью вызова функции reject(). А так как в этом случае промис завершился с ошибкой, то, следовательно, будет вызван catch().    

Пример 
```
div><button id="run">Новая попытка</button></div>
<div id="result"></div>

<script>
  let isProcess = false;
  elResult = document.querySelector('#result');

  document.querySelector('#run').onclick = () => {
    if (isProcess) {
      elResult.textContent = 'Подождите! Задача ещё выполняется!';
      return;
    }
    isProcess = true;
    elResult.textContent = 'Задача в процессе...';
    const promise = new Promise((resolve, reject) => {
      setTimeout(() => {
        const mark = Math.floor(Math.random() * 4) + 2;
        mark > 3 ? resolve(mark) : reject(mark);
      }, 5000);
    });
    promise
      .then(value => {
        elResult.textContent = `Ура! Вы сдали экзамен на ${value}! Папа, как и обещал дал вам 100$.`;
      })
      .catch(value => {
        elResult.textContent = `Увы, вы получили оценку ${value}! Папа не дал вам 100$`;
      })
      .finally(() => {
        isProcess = false;
      });
  }
</script>
```
