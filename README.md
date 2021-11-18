# script

//novo filtro search jdvtv
$(document).ready(function () {
 
    /* initially hide product list items */
    $("#jdvtv-list tr").hide();
 
    /* highlight matches text */
    var highlight = function (string) {
        $("#jdvtv-list .tr match").each(function () {
            var matchStart = $(this).text().toLowerCase().indexOf("" + string.toLowerCase() + "");
            var matchEnd = matchStart + string.length - 1;
            var beforeMatch = $(this).text().slice(0, matchStart);
            var matchText = $(this).text().slice(matchStart, matchEnd + 1);
            var afterMatch = $(this).text().slice(matchEnd + 1);
            $(this).html(beforeMatch + "<em>" + matchText + "</em>" + afterMatch);
        });
    };
 
 
    /* filter products */
    $("#search-jdvtv").on("keyup click input", function () {
        if (this.value.length > 0) {
            $("#jdvtv-list tr").removeClass("match").hide().filter(function () {
                return $(this).text().toLowerCase().indexOf($("#search-jdvtv").val().toLowerCase()) != -1;
            }).addClass("match").show();
            highlight(this.value);
            $("#jdvtv-list").show();
        }
        else {
            $("#jdvtv-list, #jdvtv-list tr").removeClass("match").hide();
        }
    });
 
 
});

/* When the user clicks on the button,
toggle between hiding and showing the dropdown content */
function myFunction() {
  document.getElementById("myDropdown").classList.toggle("show");
}

function filterFunction() {
  var input, filter, ul, li, a, i;
  input = document.getElementById("myInput");
  filter = input.value.toUpperCase();
  div = document.getElementById("myDropdown");
  a = div.getElementsByTagName("a");
  for (i = 0; i < a.length; i++) {
    txtValue = a[i].textContent || a[i].innerText;
    if (txtValue.toUpperCase().indexOf(filter) > -1) {
      a[i].style.display = "";
    } else {
      a[i].style.display = "none";
    }
  }
}


//to-do list//
const inputToggle = document.querySelector('.header__button');
const text = document.getElementById('text');
const ul = document.querySelector('.content__list');
inputToggle.addEventListener('click', function() {
  document.querySelector('.input__container').classList.toggle('hide');
  document.querySelector('.contentx').classList.toggle('hideHeight');
});

text.addEventListener('keypress', function(e) {
  if (e.keyCode === 13) {
    if (
      !text.value.trim() ||
      text.value.trim().length < 3 ||
      text.value.trim().length > 20
    ) {
      showMessage('Pelo menos 3 caracteres e 20 no máximo', '#721c24', '#f8d7da');
    } else {
      addItem(text.value);
      showMessage('Item adicionado! ', '#155724', '#d4edda');
    }
  }
});

function addItem(data) {
  document.querySelector('.content__list').innerHTML += `
<div class="wraper">
              <div class="remove">✖</div>
            <li class="content__list--item">${data}</li>
          </div>
`;
  addToLocale(data);
  count();
  text.value = null;
}

ul.addEventListener('click', e => {
  if (e.target.classList.contains('remove')) {
    e.target.parentElement.remove();
    delFromLocal2(e.target.parentElement.children[1].textContent);
    showMessage('Item Deletado!', '#004085', '#cce5ff');
  }
  if (e.target.classList.contains('content__list--item')) {
    e.target.classList.toggle('finished');
    if (e.target.classList.contains('finished')) {
      delFromLocal(e.target.parentElement.children[1].textContent, true);
      showMessage('já assisti esse!', '#004085', '#cce5ff');
    } else {
      delFromLocal(e.target.parentElement.children[1].textContent, false);
      showMessage('Ainda não assisti esse!', '#004085', '#cce5ff');
    }
    console.log('g');
  }
});

function addToLocale(data) {
  let arr = [];
  if (localStorage.getItem('todo')) {
    arr = JSON.parse(localStorage.getItem('todo'));
  }
  arr.push({ text: data, isChecked: false });
  localStorage.setItem('todo', JSON.stringify(arr));
}

document.addEventListener('DOMContentLoaded', loadfromlocal);
function loadfromlocal() {
  let data = JSON.parse(localStorage.getItem('todo'));
  document.querySelector('.content__list').innerHTML = null;
  if (data) {
    data.forEach(function(todo) {
      document.querySelector('.content__list').innerHTML += `
            <div class="wraper">
                        <div class="remove">✖</div>
                        <li class="content__list--item ${
                          todo.isChecked ? 'finished' : 'notFinished'
                        }">${todo.text}</li>
                      </div>
            `;
    });
  }
  count();
}

function delFromLocal(item, status) {
  let arr = JSON.parse(localStorage.getItem('todo'));
  if (arr) {
    for (let i = 0; i < arr.length; i++) {
      if (item === arr[i].text) {
        arr[i].isChecked = status;
        localStorage.setItem('todo', JSON.stringify(arr));
        break;
      }
    }
  }
  //   count();
}

function count() {
  let a = JSON.parse(localStorage.getItem('todo'));
  if (a) {
    document.querySelector('.footer').textContent = `Você têm ${a.length} iten(s)!`;
  } else {
    document.querySelector('.footer').textContent = `Você têm 0 iten(s)!`;
  }
}

function showMessage(message, color, bgcolor) {
  let output = document.querySelector('.message');
  output.style.backgroundColor = bgcolor;
  output.style.color = color;
  output.style.width = '100%';
  output.style.fontSize = '17px';
  output.textContent = message;
  setTimeout(() => {
    output.style.backgroundColor = 'none';
    output.style.fontSize = '0';
    output.style.width = '0';
    output.textContent = null;
  }, 3000);
}

function delFromLocal2(item) {
  let arr = JSON.parse(localStorage.getItem('todo'));
  if (arr) {
    for (let i = 0; i < arr.length; i++) {
      if (item === arr[i].text) {
        arr.splice(i, 1);
        localStorage.setItem('todo', JSON.stringify(arr));
        break;
      }
    }
  }
  count();
}
	

