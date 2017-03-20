# Replacement for buggy string concatenation!

### How to use template literals

+ Concatenation

  ```javascript
  let person = {
    name: 'Kevin',
    job: 'web developer',
    height: 179,
  };
  console.log(`${person.name} is a ${person.job} with ${person.height / 2.45} inches tall.`);
  ```

+ Multi-line

  ```javascript
  const markup = `
    <div>
      <h2>${person.name}</h2>
      <h3>${person.job}</h3>
    </div>
  `;
  ```

+ You can put a template literal inside another template literals

  ```javascript
  //Loop in the template literals
  const dogs = [
    { name: 'Snickers', age: 2 },
    { name: 'Hugo', age: 8 },
    { name: 'Sunny', age: 1 }
  ];

  <content></content>st markup = `
  <ul class="dogs">
    ${dogs.map(dog => `<li>${dog.name} is ${dog.age * 7} years old.</li>`).join('')}
  </ul>
  `;
  document.body.innerHTML = markup;
  ```

### Put logic inside template literals

+ Put **if statement** inside template literals

  ```javascript
  const song = {
    name: 'Dying to live',
    artist: 'Tupac',
    featuring: 'Biggie Smalls'
  };

  const markup = `
  <div class="song">
    <p>
      ${song.name} - ${song.artist}
      ${song.featuring ? `(Featuring ${song.feature})` : ''}
    </p>
  </div>
  `;
  ```

+ Create a render function

  ```javascript
  const beer = {
    name: 'Belgian Wit',
    brewery: 'Steam Whistle Brewery',
    keywords: ['pale', 'cloudy', 'spiced', 'crisp']
  };
  
  const renderKeywords => (keywords) {
    return `
    <ul>
      ${keywords.map(keyword => `<li>${keyword}</li>`).join('')}
    </ul>
    `;
  };

  const markup = `
    <div class="beer">
      <h2>${beer.name}</h2>
      <p class='brewery'>${beer.brewery}</p>
      ${renderKeywords(beer.keywords)}
    </div>
  `;

  document.body.innerHTML = markup;
  ```

### Advanced uses - Tagged

+ Tagged template literals

  ```javascript
  function highlight(strings, ...values){
    let str = '';
    //Using for-of loop with destructuring
    for(let [index, element] of strings.entries()){
      str += `${element} + <span class='hl'>${values[index] || ''}</span>`;
    }
    //Using array.forEach()
    strings.forEach((element, index) => {
      str += `${element} + <span class='hl'>${values[index] || ''}</span>`;
    });

    return str;
  };

  const name = 'Snickers';
  const age = 100;
  const sentence = highlight`My dog's name is ${name} and he is ${age} years old`;
  document.body.innerHTML = sentence;

  //More examples
  const dict = {
    HTML: 'Hyper Text Markup Language',
    CSS: 'Cascading Style Sheets',
    JS: 'JavaScript'
  };

  function addAbbreviations(strings, ...values) {
    const abbreviated = values.map(value => {
      if(dict[value]) {
        return `<abbr title="${dict[value]}">${value}</abbr>`;
      }
      return value;
    });

    return strings.reduce((sentence, string, i) => {
      return `${sentence}${string}${abbreviated[i] || ''}`;
    }, '');
  }

  const sentence = addAbbreviations`Hello, I love to code ${'HTML'}, ${'CSS'} and ${'JS'}`;

  const bio = document.querySelector('.bio');
  const p = document.createElement('p');
  p.innerHTML = sentence;
  bio.appendChild(p);
  ```

+ Santizing user data with tagged templates

  ```html
  <!-- Using dompurify to santize -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/dompurify/0.8.2/purify.min.js"></script>
  <script>
  function sanitize(strings, ...values) {
    const dirty = strings.reduce((prev, next, i) => `${prev}${next}${values[i] || ''}`, '');
    return DOMPurify.sanitize(dirty);
  }

  const first = 'Wes';
  const aboutMe = `I am evil <img src='http://placem.it/100/100" onload="alert('Hacked!');' />`;

  const html = sanitize`
    <h3>${first}</h3>
    <p>${aboutMe}</p>
  `;

  const bio = document.querySelector('.bio');
  bio.innerHTML = html;
  </script>
  ```
  
