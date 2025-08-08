/islamic-app/
│
├── public/
│   ├── index.html
│   ├── manifest.json
│   └── service-worker.js
│
├── src/
│   ├── App.js
│   ├── index.js
│   ├── components/
│   │   ├── Quran.js
│   │   ├── Hadith.js
│   │   ├── Duas.js
│   │   ├── PrayerTimes.js
│   │   ├── Quotes.js
│   │   ├── ScholarQA.js
│   │   └── Navigation.js
│   └── styles/
│       └── main.css
│
├── package.json
└── README.md
```

---
 `public/index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="manifest" href="/manifest.json" />
    <meta name="theme-color" content="#0a3d62" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Islamic Companion</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>
```

---

`public/manifest.json`

```json
{
  "short_name": "IslamicApp",
  "name": "Islamic Companion PWA",
  "icons": [
    {
      "src": "icon-192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "icon-512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ],
---

📁 `src/components/Quran.js`

```js
import React, { useState, useEffect } from 'react';

function Quran() {
  const [surahs, setSurahs] = useState([]);

  useEffect(() => {
    fetch('https://api.alquran.cloud/v1/surah')
      .then(res => res.json())
      .then(data => setSurahs(data.data));
  }, []);

  return (
    <div>
      <h2>📖 The Holy Qur'an</h2>
      <ul>
        {surahs.map(surah => (
          <li key={surah.number}>
            {surah.number}. {surah.englishName} ({surah.name})
          </li>
        ))}
      </ul>
    </div>
  );
}

export default Quran;
```

---

📁 `src/components/Hadith.js`

```js
import React, { useState, useEffect } from 'react';

function Hadith() {
  const [hadiths, setHadiths] = useState([]);

  useEffect(() => {
    fetch('https://api.sunnah.com/v1/collections/bukhari/books/1/hadiths', {
      headers: { 'X-API-Key': 'YOUR_API_KEY_HERE' }
    })
      .then(res => res.json())
      .then(data => setHadiths(data.data || []));
  }, []);

  return (
    <div>
      <h2>📚 Hadith Collection (Bukhari)</h2>
      <ul>
        {hadiths.map((h, idx) => (
          <li key={idx}>{h.hadith[0].body}</li>
        ))}
      </ul>
    </div>
  );
}

export default Hadith;
```

---📁 `src/components/Duas.js`

```js
import React from 'react';

const Duas = () => {
  const duas = [
    { title: 'Before Sleeping', text: 'اللَّهُمَّ بِاسْمِكَ أَمُوتُ وَأَحْيَا' },
    { title: 'Waking Up', text: 'الْـحَمْـدُ للهِ الَّذِي أَحْيَانَا' },
    { title: 'Entering Home', text: 'بِسْمِ اللَّهِ وَلَجْنَا وَبِسْمِ اللَّهِ خَرَجْنَا' }
  ];

  return (
    <div>
      <h2>🤲 Daily Duas</h2>
      <ul>
        {duas.map((dua, idx) => (
          <li key={idx}>
            <strong>{dua.title}:</strong> {dua.text}
          </li>
        ))}
      </ul>
    </div>
  );
};

export default Duas;
```

---

Say *“Continue Phase 3”* to get the next features: Prayer Times, Motivational Quotes, and Scholar Q&A.✅ *Phase 3: Component Files – Prayer Times, Motivational Quotes, Ask a Scholar*

---

📁 `src/components/PrayerTimes.js`

```js
import React, { useState, useEffect } from 'react';

function PrayerTimes() {
  const [times, setTimes] = useState(null);
  const [location, setLocation] = useState({ lat: null, lon: null });

  useEffect(() => {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(({ coords }) => {
        setLocation({ lat: coords.latitude, lon: coords.longitude });
      });
    }
  }, []);

  useEffect(() => {
    if (location.lat && location.lon) {
      fetch(
        `https://api.aladhan.com/v1/timings?latitude=location.lat   longitude={location.lon}&method=2`
      )
        .then(res => res.json())
        .then(data => setTimes(data.data.timings));
    }
  }, [location]);

  if (!times) return <p>Loading Prayer Times...</p>;

  return (
    <div>
      <h2>🕌 Prayer Times</h2>
      <ul>
        {Object.entries(times).map(([name, time]) => (
          <li key={name}>
            {name}: {time}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default PrayerTimes;
```

---

📁 `src/components/MotivationalQuotes.js`

```js
import React, { useState, useEffect } from 'react';

const quotes = ["Trust in Allah, but tie your camel.",
  "The best among you are those who have the best manners.",
  "Patience is the key to relief.",
  "Indeed, with hardship comes ease.",
  "Do good deeds, and you will be rewarded."
];

function MotivationalQuotes() {
  const [quote, setQuote] = useState('');

  useEffect(() => {
    const randomIndex = Math.floor(Math.random() * quotes.length);
    setQuote(quotes[randomIndex]);
  }, []);

  return (
    <div>
      <h2>🌟 Islamic Motivational Quote</h2>
      <p>{quote}</p>
    </div>
  );
}

export default MotivationalQuotes;
```

---

📁 `src/components/AskScholar.js`

```js
import React, { useState } from 'react';

function AskScholar() {
  const [question, setQuestion] = useState('');
  const [answers, setAnswers] = useState([]);

  const handleSubmit = e => {
    e.preventDefault();
    if (!question.trim()) return;

    // Simulated scholar answer, ideally replaced with backend or API
    const answer = `Scholar’s reply to: "${question}" — Please check your local scholar for accurate answers.`;
    setAnswers([...answers, { question, answer }]);
    setQuestion('');
  };

  return (
    <div>
      <h2>🗣 Ask a Scholar</h2>
      <form onSubmit={handleSubmit}>
        <textarea
          value={question}onChange={e => setQuestion(e.target.value)}
          placeholder="Type your question here..."
          rows="3"
          style={{ width: '100%' }}
        />
        <button type="submit">Ask</button>
      </form>
      <div>
        {answers.map((qa, idx) => (
          <div key={idx} style={{ marginTop: '10px', borderBottom: '1px solid #ccc' }}>
            <p><b>Q:</b> {qa.question}</p>
            <p><b>A:</b> {qa.answer}</p>
          </div>
        ))}
      </div>
    </div>
  );
}

export default AskScholar;
```

---

Say *“Next Phase”* to continue with Prayer Tracker, Sin Tracker, Nasheed Player, and more!✅ *Phase 4: Component Files – Prayer Tracker Calendar, Sin & Repentance Tracker, Nasheed Player*

---

📁 `src/components/PrayerTracker.js`

```js
import React, { useState, useEffect } from 'react';

function PrayerTracker() {
  const [records, setRecords] = useState(() => {
    const saved = localStorage.getItem('prayerTracker');
    return saved ? JSON.parse(saved) : {};
  });

  const togglePrayer = (date, prayer) => {
    setRecords(prev => {
      const dayRecords = prev[date] || {};
      const newDayRecords = { ...dayRecords, [prayer]: !dayRecords[prayer] };
      const updated = { ...prev, [date]: newDayRecords };
      localStorage.setItem('prayerTracker', JSON.stringify(updated));
      return updated;
    });
  };

  const today = new Date().toISOString().slice(0, 10);
  const prayers = ['Fajr', 'Dhuhr', 'Asr', 'Maghrib', 'Isha'];

  return (
    <div>
      <h2>🕌 Prayer Tracker</h2>
      <p>Date: {today}</p>
      <div>
        {prayers.map(prayer => (
          <label key={prayer} style={{ marginRight: '15px' }}>
            <input
              type="checkbox"
              checked={records[today]?.[prayer] || false}
              onChange={() => togglePrayer(today, prayer)}
            />
            {prayer}
          </label>
        ))}
      </div>
    </div>);
}

export default PrayerTracker;
```

---

📁 `src/components/SinTracker.js`

```js
import React, { useState, useEffect } from 'react';

function SinTracker() {
  const [sins, setSins] = useState(() => {
    const saved = localStorage.getItem('sinTracker');
    return saved ? JSON.parse(saved) : [];
  });

  const [newSin, setNewSin] = useState('');

  const addSin = () => {
    if (!newSin.trim()) return;
    const updated = [...sins, { id: Date.now(), text: newSin, repented: false }];
    setSins(updated);
    localStorage.setItem('sinTracker', JSON.stringify(updated));
    setNewSin('');
  };

  const toggleRepent = id => {
    const updated = sins.map(s =>
      s.id === id ? { ...s, repented: !s.repented } : s
    );
    setSins(updated);
    localStorage.setItem('sinTracker', JSON.stringify(updated));
  };

  return (
    <div>
      <h2>🙏 Sin & Repentance Tracker</h2>
      <input
        type="text"
        placeholder="Write your sin..."
        value={newSin}
        onChange={e => setNewSin(e.target.value)}
      />
      <button onClick={addSin}>Add</button>
      <ul>
        {sins.map(sin => (
          <li key={sin.id} style={{ textDecoration: sin.repented ? 'line-through' : 'none' }}>
            {sin.text}<button onClick={() => toggleRepent(sin.id)} style={{ marginLeft: '10px' }}>
              {sin.repented ? 'Undo' : 'Repented'}
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default SinTracker;
```

---

📁 `src/components/NasheedPlayer.js`

```js
import React, { useState } from 'react';

const nasheeds = [
  { id: 1, title: 'Nasheed 1', url: 'https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3' },
  { id: 2, title: 'Nasheed 2', url: 'https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3' }
];

function NasheedPlayer() {
  const [current, setCurrent] = useState(null);

  return (
    <div>
      <h2>🎵 Nasheed Player</h2>
      <ul>
        {nasheeds.map(n => (
          <li key={n.id}>
            <button onClick={() => setCurrent(n.url)}>{n.title}</button>
          </li>
        ))}
      </ul>
      {current && (
        <audio controls autoPlay src={current} style={{ width: '100%' }}>
          Your browser does not support the audio element.
        </audio>
      )}
    </div>
  );
}

export default NasheedPlayer;
```

---

Say *“Next”* when ready for the next phase!✅ *Phase 5: Component Files – Zakat Calculator, Halal & Haram Guide, Islamic Books*

---

📁 `src/components/ZakatCalculator.js`

```js
import React, { useState } from 'react';

function ZakatCalculator() {
  const [wealth, setWealth] = useState('');
  const [zakat, setZakat] = useState(null);

  const calculateZakat = () => {
    const amount = parseFloat(wealth);
    if (isNaN(amount) || amount <= 0) {
      alert('Please enter a valid amount');
      return;
    }
    // Zakat is 2.5% of wealth above nisab
    const zakatAmount = amount * 0.025;
    setZakat(zakatAmount.toFixed(2));
  };

  return (
    <div>
      <h2>💰 Zakat Calculator</h2>
      <input
        type="number"
        placeholder="Enter your total wealth"
        value={wealth}
        onChange={e => setWealth(e.target.value)}
      />
      <button onClick={calculateZakat}>Calculate</button>
      {zakat !== null && (
        <p>Your Zakat amount is: <strong>${zakat}</strong></p>
      )}
    </div>
  );
}

export default ZakatCalculator;
```

---

📁 `src/components/HalalHaramGuide.js`

```js
import React, { useState } from 'react';

const topics = [
  { id: 1, title: 'Halal Foods', content: 'Halal foods are those permitted by Islamic law...' },{ id: 2, title: 'Haram Foods', content: 'Haram foods include pork, alcohol, and blood...' },
  { id: 3, title: 'Qur’an References', content: 'Surah Al-Ma’idah (5:3) forbids certain foods...' }
];

function HalalHaramGuide() {
  const [selected, setSelected] = useState(null);

  return (
    <div>
      <h2>📜 Halal & Haram Guide</h2>
      <ul>
        {topics.map(topic => (
          <li key={topic.id}>
            <button onClick={() => setSelected(topic.id)}>{topic.title}</button>
          </li>
        ))}
      </ul>
      {selected && (
        <div style={{ marginTop: '15px' }}>
          <p>{topics.find(t => t.id === selected).content}</p>
        </div>
      )}
    </div>
  );
}

export default HalalHaramGuide;
```

---

📁 `src/components/IslamicBooks.js`

```js
import React, { useState } from 'react';

const books = [
  {
    id: 1,
    title: 'Book of Tawheed',
    author: 'Ibn Abdul Wahhab',
    excerpt: 'Tawheed means to declare the oneness of Allah...',
  },
  {
    id: 2,
    title: 'Riyadh As-Salihin',
    author: 'Imam Nawawi',
    excerpt: 'This book contains hadiths related to good manners...',
  }
];

function IslamicBooks() {
  const [selected, setSelected] = useState(null);

  return (
    <div>
      <h2>📚 Islamic Books</h2>
      <ul>{books.map(book => (
          <li key={book.id}>
            <button onClick={() => setSelected(book.id)}>{book.title} - {book.author}</button>
          </li>
        ))}
      </ul>
      {selected && (
        <div style={{ marginTop: '10px' }}>
          <p>{books.find(b => b.id === selected).excerpt}</p>
        </div>
      )}
    </div>
  );
}

export default IslamicBooks;
```

---

Say *“Next”* when ready for phase 6!✅ *Phase 6: Component Files – Inheritance Laws Guide (Fara’id), User Progress Tracker, Comparative Religion*

---

📁 `src/components/InheritanceGuide.js`

```js
import React from 'react';

const faraid = [
  {
    relation: 'Spouse',
    share: '1/4 if there are children, otherwise 1/2',
    details: 'The spouse receives a fixed share depending on the presence of children.'
  },
  {
    relation: 'Father',
    share: '1/6 if there are children, otherwise remainder',
    details: 'The father’s share varies depending on surviving children.'
  },
  {
    relation: 'Mother',
    share: '1/6 or 1/3 depending on children and siblings',
    details: 'The mother receives a fixed share, conditions apply.'
  }
];

function InheritanceGuide() {
  return (
    <div>
      <h2>📜 Inheritance Laws Guide (Fara’id)</h2>
      <ul>
        {faraid.map((item, index) => (
          <li key={index}>
            <strong>{item.relation}:</strong> {item.share} - {item.details}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default InheritanceGuide;
```

---

📁 `src/components/UserProgressTracker.js`

```js
import React, { useState, useEffect } from 'react';

function UserProgressTracker() {
  const [hasanat, setHasanat] = useState(0);

  useEffect(() => {const saved = localStorage.getItem('hasanatCount');
    if (saved) setHasanat(parseInt(saved));
  }, []);

  const addHasanat = () => {
    const newCount = hasanat + 1;
    setHasanat(newCount);
    localStorage.setItem('hasanatCount', newCount);
  };

  return (
    <div>
      <h2>📊 User Progress & Hasanat Counter</h2>
      <p>Total Hasanat: {hasanat}</p>
      <button onClick={addHasanat}>Add Hasanat</button>
    </div>
  );
}

export default UserProgressTracker;
```

---

📁 `src/components/ComparativeReligion.js`

```js
import React, { useState } from 'react';

const topics = [
  {
    id: 1,
    title: 'Bible vs Qur’an on Jesus',
    content: 'Jesus is not God — proof from the Bible: John 14:28 states “The Father is greater than I.” This supports Tawheed...'
  },
  {
    id: 2,
    title: 'Tawheed Explained',
    content: 'The oneness of Allah (Tawheed) is central to Islam and fulfilled prophecies in the Bible align with this concept...'
  }
];

function ComparativeReligion() {
  const [selected, setSelected] = useState(null);

  return (
    <div>
      <h2>📖 Comparative Religion</h2>
      <ul>
        {topics.map(t => (
          <li key={t.id}>
            <button onClick={() => setSelected(t.id)}>{t.title}</button>
          </li>
        ))}
      </ul>{selected && (
        <div style={{ marginTop: '10px' }}>
          <p>{topics.find(t => t.id === selected).content}</p>
        </div>
      )}
    </div>
  );
}

export default ComparativeReligion;
```

---

Say *“Next”* when you want phase 7!✅ *Phase 7: Main App Setup, Routing, and Service Worker (PWA Support)*

---

📁 `src/App.js`

```js
import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Quran from './components/Quran';
import HadithLibrary from './components/HadithLibrary';
import PrayerTimes from './components/PrayerTimes';
import DailyVerse from './components/DailyVerse';
import AskScholar from './components/AskScholar';
import InheritanceGuide from './components/InheritanceGuide';
import UserProgressTracker from './components/UserProgressTracker';
import ComparativeReligion from './components/ComparativeReligion';
import Navbar from './components/Navbar';

function App() {
  return (
    <Router>
      <Navbar />
      <Routes>
        <Route path="/" element={<Quran />} />
        <Route path="/hadith" element={<HadithLibrary />} />
        <Route path="/prayer-times" element={<PrayerTimes />} />
        <Route path="/daily-verse" element={<DailyVerse />} />
        <Route path="/ask-scholar" element={<AskScholar />} />
        <Route path="/inheritance" element={<InheritanceGuide />} />
        <Route path="/progress" element={<UserProgressTracker />} />
        <Route path="/comparative-religion" element={<ComparativeReligion />} />
      </Routes></Router>
  );
}

export default App;
```

---

📁 `src/components/Navbar.js`

```js
import React from 'react';
import { Link } from 'react-router-dom';

function Navbar() {
  return (
    <nav style={{ padding: '10px', backgroundColor: '#1a202c', color: 'white' }}>
      <Link to="/" style={{ margin: '10px', color: 'white' }}>Qur’an</Link>
      <Link to="/hadith" style={{ margin: '10px', color: 'white' }}>Hadith</Link>
      <Link to="/prayer-times" style={{ margin: '10px', color: 'white' }}>Prayer Times</Link>
      <Link to="/daily-verse" style={{ margin: '10px', color: 'white' }}>Daily Verse</Link>
      <Link to="/ask-scholar" style={{ margin: '10px', color: 'white' }}>Ask Scholar</Link>
      <Link to="/inheritance" style={{ margin: '10px', color: 'white' }}>Inheritance</Link>
      <Link to="/progress" style={{ margin: '10px', color: 'white' }}>Progress</Link>
      <Link to="/comparative-religion" style={{ margin: '10px', color: 'white' }}>Comparative Religion</Link>
    </nav>
  );
}

export default Navbar;
```

---

📁 `src/index.js`

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';
import * as serviceWorkerRegistration from './serviceWorkerRegistration';const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);

// Register service worker for PWA offline support
serviceWorkerRegistration.register();
```

---

📁 `src/serviceWorkerRegistration.js`

```js
// Basic service worker registration file for PWA support
const isLocalhost = Boolean(
  window.location.hostname === 'localhost' ||
    window.location.hostname === '[::1]' ||
    window.location.hostname.match(
      /^127(?:\.(?:25[0-5]|2[0-4]\d|1\d\d|1̣,̣2̣)){3}/
    )
);

export function register(config) 
  if ('serviceWorker' in navigator) 
    const publicUrl = new URL(process.env.PUBLIC_URL, window.location.href);
    if (publicUrl.origin !== window.location.origin) 
      return;
    
    window.addEventListener('load', () => 
      const swUrl = `{process.env.PUBLIC_URL}/service-worker.js`;

      if (isLocalhost) {
        checkValidServiceWorker(swUrl, config);
      } else {
        registerValidSW(swUrl, config);
      }
    });
  }
}

function registerValidSW(swUrl, config) {
  navigator.serviceWorker
    .register(swUrl)
    .then(registration => {
      registration.onupdatefound = () => {
        const installingWorker = registration.installing;
        if (!installingWorker) {
          return;
        }installingWorker.onstatechange = () => {
          if (installingWorker.state === 'installed') {
            if (navigator.serviceWorker.controller) {
              if (config && config.onUpdate) {
                config.onUpdate(registration);
              }
            } else {
              if (config && config.onSuccess) {
                config.onSuccess(registration);
              }
            }
          }
        };
      };
    })
    .catch(error => {
      console.error('Error during service worker registration:', error);
    });
}

function checkValidServiceWorker(swUrl, config) {
  fetch(swUrl)
    .then(response => {
      const contentType = response.headers.get('content-type');
      if (
        response.status === 404 ||
        (contentType != null && contentType.indexOf('javascript') === -1)
      ) {
        navigator.serviceWorker.ready.then(registration => {
          registration.unregister().then(() => {
            window.location.reload();
          });
        });
      } else {
        registerValidSW(swUrl, config);
      }
    })
    .catch(() => {
      console.log(
        'No internet connection found. App is running in offline mode.'
      );
    });
}

export function unregister() {
  if ('serviceWorker' in navigator) {installingWorker.onstatechange = () => {
          if (installingWorker.state === 'installed') {
            if (navigator.serviceWorker.controller) {
              if (config && config.onUpdate) {
                config.onUpdate(registration);
              }
            } else {
              if (config && config.onSuccess) {
                config.onSuccess(registration);
              }
            }
          }
        };
      };
    })
    .catch(error => {
      console.error('Error during service worker registration:', error);
    });
}

function checkValidServiceWorker(swUrl, config) {
  fetch(swUrl)
    .then(response => {
      const contentType = response.headers.get('content-type');
      if (
        response.status === 404 ||
        (contentType != null && contentType.indexOf('javascript') === -1)
      ) {
        navigator.serviceWorker.ready.then(registration => {
          registration.unregister().then(() => {
            window.location.reload();
          });
        });
      } else {
        registerValidSW(swUrl, config);
      }
    })
    .catch(() => {
      console.log(
        'No internet connection found. App is running in offline mode.'
      );
    });
}

export function unregister() {
  if ('serviceWorker' in navigator) {navigator.serviceWorker.ready.then(registration => {
      registration.unregister();
    });
  }
}
```
```js
import React, { useState, useEffect } from 'react';

// Sample Quran data structure (short sample, extend as needed)
import quranData from '../data/quran.json'; 

function Quran() {
  const [chapter, setChapter] = useState(1);
  const [verses, setVerses] = useState([]);

  useEffect(() => {
    // Load verses for the selected chapter
    const chapterData = quranData.chapters.find(c => c.id === chapter);
    if (chapterData) {
      setVerses(chapterData.verses);
    }
  }, [chapter]);

  return (
    <div style={{ padding: 20 }}>
      <h2>Qur'an - Chapter {chapter}</h2>
      <select value={chapter} onChange={e => setChapter(Number(e.target.value))}>
        {quranData.chapters.map(c => (
          <option key={c.id} value={c.id}>{c.name}</option>
        ))}
      </select>

      <div style={{ marginTop: 20 }}>
        {verses.map(verse => (
          <div key={verse.id} style={{ marginBottom: 10, borderBottom: '1px solid #ccc', paddingBottom: 10 }}>
            <p><strong>Arabic:</strong> {verse.arabic}</p>
            <p><strong>Transliteration:</strong> {verse.transliteration}</p>
            <p><strong>Translation:</strong> {verse.translation}</p>
          </div>
        ))}</div>
    </div>
  );
}

export default Quran;
```

---

📁 `src/data/quran.json`

```json
{
  "chapters": [
    {
      "id": 1,
      "name": "Al-Fatiha",
      "verses": [
        {
          "id": 1,
          "arabic": "بِسْمِ اللَّهِ الرَّحْمَنِ الرَّحِيمِ",
          "transliteration": "Bismillahir-Rahmanir-Raheem",
          "translation": "In the name of Allah, the Most Gracious, the Most Merciful."
        },
        {
          "id": 2,
          "arabic": "الْحَمْدُ لِلَّهِ رَبِّ الْعَالَمِينَ",
          "transliteration": "Alhamdu lillahi Rabbil 'aalameen",
          "translation": "All praise is due to Allah, Lord of the worlds."
        }
      ]
    },
    {
      "id": 2,
      "name": "Al-Baqarah",
      "verses": [
        {
          "id": 1,
          "arabic": "الم",
          "transliteration": "Alif-Lam-Mim",
          "translation": "Alif, Lam, Meem."
        },
        {
          "id": 2,
          "arabic": "ذَٰلِكَ الْكِتَابُ لَا رَيْبَ ۛ فِيهِ ۛ هُدًى لِّلْمُتَّقِينَ",
          "transliteration": "Thalikal-Kitabu la rayba feehi hudal lilmuttaqeen",
          "translation": "This is the Book about which there is no doubt, a guidance for those conscious of Allah."
        }
      ]
    }
  ]
}
```

---

*Notes:*- The Quran content is stored locally in a JSON file for full offline access.
- You can expand `quran.json` with the full Quran data (there are open-source repositories available).
- User can select chapters to view verses with Arabic, transliteration, and translation.
- UI is minimal and can be enhanced with styles and pagination later.

---

Say *“Next”* for the next part or if you want me to add more features to this component!✅ *Phase 9: Hadith Library Component with Offline Support*

---

📁 `src/components/Hadith.js`

```js
import React, { useState, useEffect } from 'react';
import hadithData from '../data/hadith.json';

function Hadith() {
  const [collection, setCollection] = useState('Bukhari');
  const [hadiths, setHadiths] = useState([]);

  useEffect(() => {
    // Load hadiths from selected collection
    setHadiths(hadithData[collection] || []);
  }, [collection]);

  return (
    <div style={{ padding: 20 }}>
      <h2>Hadith Library</h2>
      <select value={collection} onChange={e => setCollection(e.target.value)}>
        {Object.keys(hadithData).map(col => (
          <option key={col} value={col}>{col}</option>
        ))}
      </select>

      <div style={{ marginTop: 20 }}>
        {hadiths.length === 0 ? (
          <p>No Hadith found in this collection.</p>
        ) : (
          hadiths.map(h => (
            <div key={h.id} style={{ marginBottom: 15, borderBottom: '1px solid #ccc', paddingBottom: 10 }}>
              <p><strong>Arabic:</strong> {h.arabic}</p>
              <p><strong>Translation:</strong> {h.translation}</p>
              <p><em>Reference: {h.reference}</em></p>
            </div>
          ))
        )}
      </div>
    </div>
  );
}

export default Hadith;```

---

📁 `src/data/hadith.json`

```json
{
  "Bukhari": [
    {
      "id": 1,
      "arabic": "إِنَّمَا الْأَعْمَالُ بِالنِّيَّاتِ",
      "translation": "Actions are judged by intentions.",
      "reference": "Sahih Bukhari, Hadith 1"
    },
    {
      "id": 2,
      "arabic": "الدِّينُ النَّصِيحَةُ",
      "translation": "Religion is sincere advice.",
      "reference": "Sahih Bukhari"
    }
  ],
  "Muslim": [
    {
      "id": 1,
      "arabic": "لاَ يُؤْمِنُ أَحَدُكُمْ حَتَّى يُحِبَّ لأَخِيهِ مَا يُحِبُّ لِنَفْسِهِ",
      "translation": "None of you truly believes until he loves for his brother what he loves for himself.",
      "reference": "Sahih Muslim"
    }
  ]
}
```

---

*Notes:*

- Hadith collections are saved locally for offline access.
- User selects from collections (Bukhari, Muslim, etc.).
- Each hadith displays Arabic text, translation, and reference.
- The JSON file can be expanded with more hadiths.

---

Reply *“Next”* to get the next phase or let me know if you want me to add anything here!```javascript
// Phase 10: AskAScholar Component (Online Q&A with Comment System)

import React, { useState, useEffect } from 'react';

const AskAScholar = () => {
  const [questions, setQuestions] = useState([]);
  const [newQuestion, setNewQuestion] = useState('');
  const [loading, setLoading] = useState(true);

  // Simulated fetch from backend (replace with real API call)
  useEffect(() => {
    const fetchQuestions = async () => {
      // Simulate network delay
      setLoading(true);
      const savedQuestions = JSON.parse(localStorage.getItem('questions')) || [];
      setQuestions(savedQuestions);
      setLoading(false);
    };
    fetchQuestions();
  }, []);

  const submitQuestion = () => {
    if (newQuestion.trim() === '') return;
    const updatedQuestions = [
      ...questions,
      { id: Date.now(), text: newQuestion, answers: [] },
    ];
    setQuestions(updatedQuestions);
    localStorage.setItem('questions', JSON.stringify(updatedQuestions));
    setNewQuestion('');
  };

  const addAnswer = (id, answerText) => {
    if (answerText.trim() === '') return;
    const updatedQuestions = questions.map(q => {
      if (q.id === id) {
        return { ...q, answers: [...q.answers, { id: Date.now(), text: answerText }] };
      }
      return q;
    });setQuestions(updatedQuestions);
    localStorage.setItem('questions', JSON.stringify(updatedQuestions));
  };

  return (
    <div style={{ padding: 20 }}>
      <h2>Ask a Scholar</h2>
      <textarea
        rows="3"
        placeholder="Type your question here..."
        value={newQuestion}
        onChange={e => setNewQuestion(e.target.value)}
        style={{ width: '100%', marginBottom: 10 }}
      />
      <button onClick={submitQuestion}>Submit Question</button>

      {loading ? (
        <p>Loading questions...</p>
      ) : (
        <div style={{ marginTop: 20 }}>
          {questions.length === 0 ? (
            <p>No questions yet. Be the first to ask!</p>
          ) : (
            questions.map(q => <QuestionCard key={q.id} question={q} addAnswer={addAnswer} />)
          )}
        </div>
      )}
    </div>
  );
};

const QuestionCard = ({ question, addAnswer }) => {
  const [answerText, setAnswerText] = useState('');
  const [showAnswerInput, setShowAnswerInput] = useState(false);

  return (
    <div style={{ border: '1px solid #ccc', padding: 15, marginBottom: 15 }}>
      <p><strong>Q:</strong> {question.text}</p>
      <button onClick={() => setShowAnswerInput(!showAnswerInput)}>
        {showAnswerInput ? 'Cancel' : 'Add Answer'}
      </button>{showAnswerInput && (
        <div style={{ marginTop: 10 }}>
          <textarea
            rows="2"
            placeholder="Write your answer..."
            value={answerText}
            onChange={e => setAnswerText(e.target.value)}
            style={{ width: '100%', marginBottom: 10 }}
          />
          <button
            onClick={() => {
              addAnswer(question.id, answerText);
              setAnswerText('');
              setShowAnswerInput(false);
            }}
          >
            Submit Answer
          </button>
        </div>
      )}

      {question.answers.length > 0 && (
        <div style={{ marginTop: 10 }}>
          <strong>Answers:</strong>
          <ul>
            {question.answers.map(ans => (
              <li key={ans.id}>{ans.text}</li>
            ))}
          </ul>
        </div>
      )}
    </div>
  );
};

export default AskAScholar;
```
Let me know if you want me to send the next phase!```javascript
// Phase 11: PrayerTrackerCalendar Component

import React, { useState, useEffect } from 'react';

const PrayerTrackerCalendar = () => {
  const [prayers, setPrayers] = useState({}); // { '2025-08-06': { Fajr: true, Dhuhr: false, ... } }
  const [selectedDate, setSelectedDate] = useState(new Date().toISOString().slice(0, 10));

  useEffect(() => {
    const saved = JSON.parse(localStorage.getItem('prayerTracker')) || {};
    setPrayers(saved);
  }, []);

  useEffect(() => {
    localStorage.setItem('prayerTracker', JSON.stringify(prayers));
  }, [prayers]);

  const togglePrayer = (date, prayerName) => {
    setPrayers(prev => {
      const dayPrayers = prev[date] || {};
      return {
        ...prev,
        [date]: {
          ...dayPrayers,
          [prayerName]: !dayPrayers[prayerName],
        },
      };
    });
  };

  const prayersList = ['Fajr', 'Dhuhr', 'Asr', 'Maghrib', 'Isha'];

  return (
    <div style={{ padding: 20 }}>
      <h2>Prayer Tracker Calendar</h2>
      <input
        type="date"
        value={selectedDate}
        onChange={e => setSelectedDate(e.target.value)}
        style={{ marginBottom: 20 }}
      />
      <div>
        {prayersList.map(prayer => {
          const done = prayers[selectedDate]?.[prayer] || false;```javascript
// Phase 12: SinAndRepentanceTracker Component

import React, { useState, useEffect } from 'react';

const SinAndRepentanceTracker = () => {
  const [sins, setSins] = useState([]);
  const [newSin, setNewSin] = useState('');
  const [repentanceCount, setRepentanceCount] = useState(0);

  useEffect(() => {
    const storedSins = JSON.parse(localStorage.getItem('sinsList')) || [];
    const storedRepentance = JSON.parse(localStorage.getItem('repentanceCount')) || 0;
    setSins(storedSins);
    setRepentanceCount(storedRepentance);
  }, []);

  useEffect(() => {
    localStorage.setItem('sinsList', JSON.stringify(sins));
    localStorage.setItem('repentanceCount', JSON.stringify(repentanceCount));
  }, [sins, repentanceCount]);

  const addSin = () => {
    if (newSin.trim() === '') return;
    setSins([...sins, { id: Date.now(), text: newSin, repented: false }]);
    setNewSin('');
  };

  const markRepented = (id) => {
    setSins(prev =>
      prev.map(sin =>
        sin.id === id ? { ...sin, repented: true } : sin
      )
    );
    setRepentanceCount(prev => prev + 1);
  };

  const unmarkRepented = (id) => {
    setSins(prev =>
      prev.map(sin =>
        sin.id === id ? { ...sin, repented: false } : sin
      )
    );setRepentanceCount(prev => (prev > 0 ? prev - 1 : 0));
  };

  return (
    <div style={{ padding: 20 }}>
      <h2>Sin & Repentance Tracker</h2>
      <input
        type="text"
        value={newSin}
        placeholder="Add a sin to track"
        onChange={e => setNewSin(e.target.value)}
        style={{ width: '70%', marginRight: 10 }}
      />
      <button onClick={addSin}>Add</button>
      <ul style={{ marginTop: 20 }}>
        {sins.map(({ id, text, repented }) => (
          <li key={id} style={{ marginBottom: 10 }}>
            <span style={{ textDecoration: repented ? 'line-through' : 'none' }}>{text}</span>
            {!repented ? (
              <button onClick={() => markRepented(id)} style={{ marginLeft: 10 }}>
                Mark Repented
              </button>
            ) : (
              <button onClick={() => unmarkRepented(id)} style={{ marginLeft: 10 }}>
                Undo Repentance
              </button>
            )}
          </li>
        ))}
      </ul>
      <div style={{ marginTop: 30 }}>
        <strong>Total Repentances: {repentanceCount}</strong>
      </div>
    </div>
  );
};

export default SinAndRepentanceTracker;
```
Ready for phase 13?```javascript
// Phase 10: AskAScholar Component (Online Feature)

import React, { useState, useEffect } from 'react';

const AskAScholar = () => {
  const [questions, setQuestions] = useState([]);
  const [newQuestion, setNewQuestion] = useState('');
  const [comments, setComments] = useState({});
  const [loading, setLoading] = useState(false);

  // Load questions from localStorage to simulate server storage
  useEffect(() => {
    const savedQuestions = JSON.parse(localStorage.getItem('questions')) || [];
    const savedComments = JSON.parse(localStorage.getItem('comments')) || {};
    setQuestions(savedQuestions);
    setComments(savedComments);
  }, []);

  useEffect(() => {
    localStorage.setItem('questions', JSON.stringify(questions));
    localStorage.setItem('comments', JSON.stringify(comments));
  }, [questions, comments]);

  const submitQuestion = () => {
    if (!newQuestion.trim()) return;
    setLoading(true);
    setTimeout(() => {
      const newQ = { id: Date.now(), text: newQuestion.trim(), date: new Date().toISOString() };
      setQuestions([newQ, ...questions]);
      setNewQuestion('');
      setLoading(false);
    }, 1000); // Simulate network delay
  };

  const addComment = (questionId, commentText) => {
    if (!commentText.trim()) return;const prevIndex = (currentIndex - 1 + nasheeds.length) % nasheeds.length;
    setCurrentNasheed(nasheeds[prevIndex]);
    setIsPlaying(true);
  };

  return (
    <div style={{ padding: 20 }}>
      <h2>Nasheed Player</h2>
      <div>
        <strong>{currentNasheed.title}</strong> by {currentNasheed.artist}
      </div>
      <audio ref={audioRef} src={currentNasheed.src} />
      <div style={{ marginTop: 10 }}>
        <button onClick={handlePrev}>Prev</button>
        <button onClick={handlePlayPause} style={{ margin: "0 10px" }}>
          {isPlaying ? "Pause" : "Play"}
        </button>
        <button onClick={handleNext}>Next</button>
      </div>
    </div>
  );
};

export default NasheedPlayer;
```
Ready for phase 15? Here is *Phase 15* of your app — the *Nasheed Player (Islamic Songs)* component with offline MP3 support (simulated with static files):

```javascript
// Phase 15: NasheedPlayer Component

import React, { useState } from 'react';

const nasheeds = [
  {
    id: 1,
    title: 'Hasbi Rabbi',
    artist: 'Sami Yusuf',
    file: '/nasheeds/hasbi-rabbi.mp3',
  },
  {
    id: 2,
    title: 'Ya Ilahi',
    artist: 'Mesut Kurtis',
    file: '/nasheeds/ya-ilahi.mp3',
  },
  {
    id: 3,
    title: '99 Names of Allah',
    artist: 'Native Deen',
    file: '/nasheeds/99-names.mp3',
  },
];

const NasheedPlayer = () => {
  const [current, setCurrent] = useState(null);
  const [isPlaying, setIsPlaying] = useState(false);
  const audioRef = React.useRef(null);

  const playNasheed = (nasheed) => {
    if (current?.id === nasheed.id && isPlaying) {
      audioRef.current.pause();
      setIsPlaying(false);
    } else {
      setCurrent(nasheed);
      setIsPlaying(true);
      setTimeout(() => {
        audioRef.current.play();
      }, 100);
    }
  };

  return (
    <div style={{ padding: 20 }}>
      <h2>🎵 Nasheed Player</h2>
      <ul>
        {nasheeds.map((nasheed) => (
          <li key={nasheed.id} style={{ marginBottom: 10 }}><strong>{nasheed.title}</strong> — {nasheed.artist}
            <button
              onClick={() => playNasheed(nasheed)}
              style={{ marginLeft: 10 }}
            >
              {current?.id === nasheed.id && isPlaying ? '⏸ Pause' : '▶️ Play'}
            </button>
          </li>
        ))}
      </ul>

      {current && (
        <audio ref={audioRef} src={current.file} onEnded={() => setIsPlaying(false)} />
      )}
    </div>
  );
};

export default NasheedPlayer;
```

*Note*: Make sure your `/nasheeds/` folder is in the *public directory* and contains the MP3 files.

Let me know when you're ready for *Phase 16: Zakat Calculator & Guide*.Here is *Phase 16* of your app — the *Zakat Calculator & Guide* component:

```javascript
// Phase 16: ZakatCalculator Component

import React, { useState } from 'react';

const ZakatCalculator = () => {
  const [goldValue, setGoldValue] = useState('');
  const [cashValue, setCashValue] = useState('');
  const [otherAssets, setOtherAssets] = useState('');
  const [zakat, setZakat] = useState(null);

  const calculateZakat = () => {
    const totalAssets =
      parseFloat(goldValue || 0) +
      parseFloat(cashValue || 0) +
      parseFloat(otherAssets || 0);

    const zakatAmount = totalAssets * 0.025;
    setZakat(zakatAmount);
  };

  return (
    <div style={{ padding: 20 }}>
      <h2>🧮 Zakat Calculator</h2>
      <p>
        Zakat is 2.5% of your total qualifying assets held for a lunar year. Enter your amounts below:
      </p>

      <label>Gold/Silver (in your local currency):</label>
      <input
        type="number"
        value={goldValue}
        onChange={(e) => setGoldValue(e.target.value)}
        placeholder="e.g. 1000"
      /><br />

      <label>Cash & Savings:</label>
      <input
        type="number"
        value={cashValue}
        onChange={(e) => setCashValue(e.target.value)}
        placeholder="e.g. 2000"
      /><br /><label>Other Assets:</label>
      <input
        type="number"
        value={otherAssets}
        onChange={(e) => setOtherAssets(e.target.value)}
        placeholder="e.g. 500"
      /><br />

      <button onClick={calculateZakat} style={{ marginTop: 10 }}>
        Calculate Zakat
      </button>

      {zakat !== null && (
        <div style={{ marginTop: 20 }}>
          <h3>Your Zakat: {zakat.toFixed(2)}</h3>
        </div>
      )}

      <div style={{ marginTop: 30 }}>
        <h3>Zakat Guide</h3>
        <ul>
          <li>Pay Zakat once every lunar year on wealth above Nisab.</li>
          <li>Nisab is approximately equal to the value of 85g of gold or 595g of silver.</li>
          <li>Zakat purifies your wealth and helps support the poor and needy.</li>
          <li>Eligible items: savings, business inventory, investment profits, etc.</li>
        </ul>
      </div>
    </div>
  );
};

export default ZakatCalculator;
```
```javascript
// Phase 17: HalalHaramGuide Component

import React, { useState } from 'react';

const halalHaramData = [
  {
    type: 'Halal',
    item: 'Earning through honest trade',
    proof: '“...Allah has permitted trade and has forbidden interest.” (Surah Al-Baqarah 2:275)',
  },
  {
    type: 'Haram',
    item: 'Alcohol consumption',
    proof: '“Intoxicants and gambling... are abominations of Satan’s handiwork.” (Surah Al-Ma’idah 5:90)',
  },
  {
    type: 'Halal',
    item: 'Eating lawful meat (properly slaughtered)',
    proof: '“Eat of the good things which We have provided for you.” (Surah Al-Baqarah 2:172)',
  },
  {
    type: 'Haram',
    item: 'Riba (interest)',
    proof: '“Those who consume interest cannot stand [on the Day of Resurrection]...” (Surah Al-Baqarah 2:275)',
  },
  {
    type: 'Halal',
    item: 'Marriage to a righteous spouse',
    proof: '“And among His signs is that He created for you spouses from among yourselves...” (Surah Ar-Rum 30:21)',
  },
  {
    type: 'Haram',
    item: 'Zina (fornication/adultery)',
    proof: '“Do not approach unlawful sexual intercourse. Indeed, it is ever an immorality...” (Surah Al-Isra 17:32)',
  },
];const HalalHaramGuide = () => {
  const [filter, setFilter] = useState('All');

  const filteredData =
    filter === 'All'
      ? halalHaramData
      : halalHaramData.filter((item) => item.type === filter);

  return (
    <div style={{ padding: 20 }}>
      <h2>🕌 Halal & Haram Guide</h2>
      <p>Learn what is Halal (lawful) and Haram (forbidden) with Qur'anic references.</p>

      <div style={{ marginBottom: 10 }}>
        <button onClick={() => setFilter('All')}>All</button>
        <button onClick={() => setFilter('Halal')} style={{ marginLeft: 10 }}>
          Halal Only
        </button>
        <button onClick={() => setFilter('Haram')} style={{ marginLeft: 10 }}>
          Haram Only
        </button>
      </div>

      <ul>
        {filteredData.map((item, index) => (
          <li key={index} style={{ marginBottom: 15 }}>
            <strong>{item.type}:</strong> {item.item}
            <br />
            <em>Qur’an: {item.proof}</em>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default HalalHaramGuide;
```
```javascript
// Phase 18: IslamicBooks Component

import React, { useState } from 'react';

const books = [
  {
    title: 'Riyadh-us-Saliheen',
    description: 'A collection of authentic Hadith compiled by Imam Nawawi, covering topics like manners, worship, and morals.',
    link: '/books/riyadh-us-saliheen.pdf',
  },
  {
    title: 'Fortress of the Muslim',
    description: 'A daily supplication book with Arabic, translation, and transliteration for every dua.',
    link: '/books/fortress-of-the-muslim.pdf',
  },
  {
    title: 'Tafsir Ibn Kathir',
    description: 'A detailed explanation of the Qur’an based on authentic sources.',
    link: '/books/tafsir-ibn-kathir.pdf',
  },
  {
    title: '40 Hadith of Imam Nawawi',
    description: 'Short hadiths with big meanings, with Arabic, English translation and transliteration.',
    link: '/books/40-hadith.pdf',
  },
];

const IslamicBooks = () => {
  const [search, setSearch] = useState('');

  const filteredBooks = books.filter((book) =>
    book.title.toLowerCase().includes(search.toLowerCase())
  );

  return (
    <div style={{ padding: 20 }}>
      <h2>📚 Islamic Books Library</h2><p>Access Islamic books with English translation and Arabic transliteration.</p>

      <input
        type="text"
        placeholder="Search book title..."
        value={search}
        onChange={(e) => setSearch(e.target.value)}
        style={{ marginBottom: 15, padding: 5, width: '100%' }}
      />

      <ul>
        {filteredBooks.map((book, index) => (
          <li key={index} style={{ marginBottom: 20 }}>
            <h4>{book.title}</h4>
            <p>{book.description}</p>
            <a href={book.link} download target="_blank" rel="noreferrer">Download PDF</a>
          </li>
        ))}
      </ul>
    </div>
  );
};

export default IslamicBooks;
```

```javascript
// Phase 19: InheritanceGuide Component

import React from 'react';

const inheritanceRules = [
  {
    title: 'Parents',
    description: 'Each parent receives 1/6 if the deceased has children. If no children, the mother gets 1/3 and father receives the rest after other obligations.',
  },
  {
    title: 'Wife',
    description: 'Wife receives 1/8 if the deceased has children, or 1/4 if no children.',
  },
  {
    title: 'Husband',
    description: 'Husband receives 1/4 if the deceased has children, or 1/2 if no children.',
  },
  {
    title: 'Children',
    description: 'Sons get double the share of daughters. All shares are calculated after debts and wills.',
  },
];

const InheritanceGuide = () => {
  return (
    <div style={{ padding: 20 }}>
      <h2>📖 Islamic Inheritance (Fara’id) Guide</h2>
      <p>This section provides basic guidance on Islamic inheritance laws based on the Qur’an and Sunnah. Always consult a qualified scholar for complex cases.</p>
      <ul>
        {inheritanceRules.map((rule, index) => (
          <li key={index} style={{ marginBottom: 20 }}>
            <h4>{rule.title}</h4>
            <p>{rule.description}</p>
          </li>
        ))}
      </ul><p style={{ marginTop: 20 }}>
        ⚖️ These are general rules and do not account for specific family structures or debts. Use a full calculator or consult with a Mufti for detailed guidance.
      </p>
    </div>
  );
};

export default InheritanceGuide;
```

Let me know when you're ready for *Phase 20: User Progress Tracker + Hasanat Counter*. Sure! Here’s *Phase 20* code focused on *User Progress Tracker + Hasanat Counter* in React Native with simple state management and local storage for offline use:

```javascript
// src/components/ProgressTracker.js
import React, { useState, useEffect } from 'react';
import { View, Text, Button, StyleSheet, AsyncStorage } from 'react-native';

const ProgressTracker = () => {
  const [hasanat, setHasanat] = useState(0);

  useEffect(() => {
    loadHasanat();
  }, []);

  const loadHasanat = async () => {
    try {
      const savedHasanat = await AsyncStorage.getItem('hasanatCount');
      if (savedHasanat !== null) setHasanat(parseInt(savedHasanat));
    } catch (error) {
      console.log('Error loading hasanat:', error);
    }
  };

  const incrementHasanat = async () => {
    try {
      const newCount = hasanat + 1;
      setHasanat(newCount);
      await AsyncStorage.setItem('hasanatCount', newCount.toString());
    } catch (error) {
      console.log('Error saving hasanat:', error);
    }
  };

  const resetHasanat = async () => {
    try {
      setHasanat(0);
      await AsyncStorage.setItem('hasanatCount', '0');
    } catch (error) {
      console.log('Error resetting hasanat:', error);
    }
  };

  return (
    <View style={styles.container}><Text style={styles.title}>Hasanat Counter</Text>
      <Text style={styles.count}>{hasanat}</Text>
      <Button title="Add Hasanat" onPress={incrementHasanat} />
      <Button title="Reset" onPress={resetHasanat} color="red" />
    </View>
  );
};

const styles = StyleSheet.create({
  container: { padding: 20, alignItems: 'center' },
  title: { fontSize: 24, marginBottom: 10 },
  count: { fontSize: 48, marginBottom: 20, color: '#4caf50' },
});

export default ProgressTracker;
```

*Explanation:*

- Uses React Native hooks to manage the count.
- Stores count locally on device (AsyncStorage) so it works offline.
- Buttons to increment and reset the hasanat counter.

Would you like me to continue with *Phase 21: Comparative Religion Section* or help with integration instructions?Here’s *Phase 21: Comparative Religion Section* — a simple React Native component comparing Bible and Qur’an verses with a basic UI:

```javascript
// src/components/ComparativeReligion.js
import React, { useState } from 'react';
import { View, Text, TouchableOpacity, StyleSheet, ScrollView } from 'react-native';

const data = [
  {
    id: 1,
    title: 'Jesus is not God – Proof from Bible',
    bibleVerse: `"John 14:28 - "The Father is greater than I."`,
    quranVerse: `"Quran 4:171 - "Jesus is only a messenger of Allah..."`,
    explanation: 'This shows the difference in understanding of Jesus’s divinity.'
  },
  {
    id: 2,
    title: 'Tawheed and Biblical Prophecy',
    bibleVerse: `"Deuteronomy 6:4 - "Hear, O Israel: The Lord our God, the Lord is one."`,
    quranVerse: `"Quran 112:1 - "Say, He is Allah, [who is] One."`,
    explanation: 'Both scriptures emphasize the oneness of God.'
  },
];

const ComparativeReligion = () => {
  const [selected, setSelected] = useState(null);

  return (
    <ScrollView style={styles.container}>
      {data.map((item) => (
        <TouchableOpacity
          key={item.id}
          style={styles.card}
          onPress={() => setSelected(selected === item.id ? null : item.id)}
        ><Text style={styles.title}>{item.title}</Text>
          {selected === item.id && (
            <View style={styles.content}>
              <Text style={styles.verseTitle}>Bible Verse:</Text>
              <Text style={styles.verse}>{item.bibleVerse}</Text>
              <Text style={styles.verseTitle}>Quran Verse:</Text>
              <Text style={styles.verse}>{item.quranVerse}</Text>
              <Text style={styles.explanation}>{item.explanation}</Text>
            </View>
          )}
        </TouchableOpacity>
      ))}
    </ScrollView>
  );
};

const styles = StyleSheet.create({
  container: { padding: 10 },
  card: {
    backgroundColor: '#f0f0f0',
    marginBottom: 10,
    padding: 15,
    borderRadius: 8,
  },
  title: { fontSize: 18, fontWeight: 'bold' },
  content: { marginTop: 10 },
  verseTitle: { fontWeight: '600', marginTop: 5 },
  verse: { fontStyle: 'italic', marginVertical: 3 },
  explanation: { marginTop: 8, color: '#555' },
});

export default ComparativeReligion;
```

*What this does:*

- Displays a list of key topics comparing Bible and Qur’an verses.
- Tap each item to expand and see detailed verses and explanation.
- Scrollable for multiple entries.

Want me to keep going with the next phase or help with setup instructions?Here’s the initial Expo React Native project setup with basic navigation and placeholder screens for your app features. Just copy this into Replit or your local environment.

---

*App.js*

```javascript
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import HomeScreen from './screens/HomeScreen';
import QuranScreen from './screens/QuranScreen';
import HadithScreen from './screens/HadithScreen';
import PrayerTimesScreen from './screens/PrayerTimesScreen';

const Stack = createStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Quran" component={QuranScreen} />
        <Stack.Screen name="Hadith" component={HadithScreen} />
        <Stack.Screen name="PrayerTimes" component={PrayerTimesScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}
```

---

*/screens/HomeScreen.js*

```javascript
import React from 'react';
import { View, Text, Button, StyleSheet } from 'react-native';

export default function HomeScreen({ navigation }) {
  return (
    <View style={styles.container}>container: { flex:1, justifyContent:'center', alignItems:'center' },
  text: { fontSize:18 }
});
```

---

*/screens/PrayerTimesScreen.js*

```javascript
import React from 'react';
import { View, Text, StyleSheet } from 'react-native';

export default function PrayerTimesScreen() {
  return (
    <View style={styles.container}>
      <Text style={styles.text}>Prayer Times will be shown here.</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex:1, justifyContent:'center', alignItems:'center' },
  text: { fontSize:18 }
});
```

---

*Instructions:*

- Put each screen file inside a `/screens` folder.  
- Run `npm install @react-navigation/native @react-navigation/stack react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view` to install navigation dependencies if local.  
- On Replit, just paste this and run.  

This is phase 1 — basic app navigation and structure. Next, I’ll help you add Quran content, audio, Hadith, etc.  

Ready for the next phase?Phase 2: Adding Quran Content with Arabic, Transliteration, and Translation

---

*1. Create a data file for Quran verses*  
Create `/data/quranData.js` to hold sample verses:

```javascript
export const quranVerses = [
  {
    surah: 1,
    ayah: 1,
    arabic: "بِسْمِ اللَّهِ الرَّحْمَٰنِ الرَّحِيمِ",
    transliteration: "Bismillah ir-Rahman ir-Rahim",
    translation: "In the name of Allah, the Most Gracious, the Most Merciful."
  },
  {
    surah: 1,
    ayah: 2,
    arabic: "الْحَمْدُ لِلَّهِ رَبِّ الْعَالَمِينَ",
    transliteration: "Alhamdu lillahi Rabbil 'alamin",
    translation: "Praise be to Allah, the Lord of the Worlds."
  },
  // Add more verses as needed
];
```

---

*2. Update `/screens/QuranScreen.js` to display verses with tabs*

```javascript
import React, { useState } from 'react';
import { View, Text, FlatList, TouchableOpacity, StyleSheet } from 'react-native';
import { quranVerses } from '../data/quranData';

export default function QuranScreen() {
  const [displayMode, setDisplayMode] = useState('arabic'); // 'arabic' | 'transliteration' | 'translation'

  const renderItem = ({ item }) => (
    <View style={styles.verseContainer}>
      {displayMode === 'arabic' && <Text style={styles.arabicText}>{item.arabic}</Text>}{displayMode === 'transliteration' && <Text style={styles.transliterationText}>{item.transliteration}</Text>}
      {displayMode === 'translation' && <Text style={styles.translationText}>{item.translation}</Text>}
    </View>
  );

  return (
    <View style={styles.container}>
      <View style={styles.tabContainer}>
        <TouchableOpacity onPress={() => setDisplayMode('arabic')} style={[styles.tabButton, displayMode === 'arabic' && styles.activeTab]}>
          <Text>Arabic</Text>
        </TouchableOpacity>
        <TouchableOpacity onPress={() => setDisplayMode('transliteration')} style={[styles.tabButton, displayMode === 'transliteration' && styles.activeTab]}>
          <Text>Transliteration</Text>
        </TouchableOpacity>
        <TouchableOpacity onPress={() => setDisplayMode('translation')} style={[styles.tabButton, displayMode === 'translation' && styles.activeTab]}>
          <Text>Translation</Text>
        </TouchableOpacity>
      </View>

      <FlatList
        data={quranVerses}
        keyExtractor={item => `item.surah-{item.ayah}`}
        renderItem={renderItem}
        style={{ flex: 1 }}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 10 },tabContainer: { flexDirection: 'row', justifyContent: 'space-around', marginBottom: 10 },
  tabButton: { padding: 10, borderWidth: 1, borderRadius: 5 },
  activeTab: { backgroundColor: '#ddd' },
  verseContainer: { marginBottom: 15 },
  arabicText: { fontSize: 24, textAlign: 'right', fontFamily: 'Arial' },
  transliterationText: { fontSize: 18, fontStyle: 'italic' },
  translationText: { fontSize: 16, color: '#333' },
});
```

---

*Summary:*  
- Added a small Quran dataset.  
- Created tab buttons to toggle between Arabic, transliteration, and translation views.  
- Display verses in a scrollable list.

---

Ready for phase 3? We can add audio recitations next!Phase 3: Adding 50+ World-Famous Reciters (MP3 support)

---

*1. Create a reciters data file `/data/reciters.js`:*

```javascript
export const reciters = [
  {
    id: '1',
    name: 'Abdul Basit',
    audioUrl: 'https://example.com/audio/abdul_basit.mp3',
  },
  {
    id: '2',
    name: 'Mishary Rashid',
    audioUrl: 'https://example.com/audio/mishary_rashid.mp3',
  },
  // Add more reciters here
];
```

---

*2. Add a new screen `/screens/RecitersScreen.js` to list reciters and play audio:*

```javascript
import React, { useState } from 'react';
import { View, Text, FlatList, TouchableOpacity, StyleSheet } from 'react-native';
import { Audio } from 'expo-av';
import { reciters } from '../data/reciters';

export default function RecitersScreen() {
  const [sound, setSound] = useState(null);
  const [playingId, setPlayingId] = useState(null);

  async function playSound(audioUrl, id) {
    if (sound) {
      await sound.unloadAsync();
      setPlayingId(null);
    }
    const { sound: newSound } = await Audio.Sound.createAsync({ uri: audioUrl });
    setSound(newSound);
    setPlayingId(id);
    await newSound.playAsync();
    newSound.setOnPlaybackStatusUpdate((status) => {
      if (!status.isPlaying) {
        setPlayingId(null);
      }
    });
  }const renderItem = ({ item }) => (
    <TouchableOpacity style={styles.reciterItem} onPress={() => playSound(item.audioUrl, item.id)}>
      <Text style={styles.reciterName}>{item.name}</Text>
      {playingId === item.id && <Text style={styles.playing}>Playing...</Text>}
    </TouchableOpacity>
  );

  return (
    <View style={styles.container}>
      <FlatList
        data={reciters}
        keyExtractor={(item) => item.id}
        renderItem={renderItem}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 10 },
  reciterItem: { padding: 15, borderBottomWidth: 1, borderColor: '#ccc' },
  reciterName: { fontSize: 18 },
  playing: { color: 'green', fontSize: 14, marginTop: 5 },
});
```

---

*3. Add audio permissions and dependencies*

If using Expo, add `expo-av`:

```bash
expo install expo-av
```

---

*Summary:*  
- Added a reciters list with their audio URLs.  
- Allows users to play recitations.  
- Displays "Playing..." indicator for active audio.

---

Ready for phase 4? We can add Prayer Times with Location-Based Accuracy next!Phase 4: Prayer Times with Location-Based Accuracy

---

*1. Install dependencies for location and prayer time calculation*

If using Expo:

```bash
expo install expo-location
npm install islamic-prayer-times
```

---

*2. Create `/screens/PrayerTimesScreen.js`*

```javascript
import React, { useEffect, useState } from 'react';
import { View, Text, ActivityIndicator, StyleSheet } from 'react-native';
import * as Location from 'expo-location';
import PrayerTimes from 'islamic-prayer-times';

export default function PrayerTimesScreen() {
  const [location, setLocation] = useState(null);
  const [errorMsg, setErrorMsg] = useState(null);
  const [prayerTimes, setPrayerTimes] = useState(null);

  useEffect(() => {
    (async () => {
      let { status } = await Location.requestForegroundPermissionsAsync();
      if (status !== 'granted') {
        setErrorMsg('Permission to access location was denied');
        return;
      }

      let loc = await Location.getCurrentPositionAsync({});
      setLocation(loc.coords);

      // Calculate prayer times for today
      const date = new Date();
      const times = PrayerTimes.getPrayerTimes(date, [loc.coords.latitude, loc.coords.longitude], 'auto', 5);
      setPrayerTimes(times);
    })();
  }, []);

  if (errorMsg) {
    return (<View style={styles.container}>
        <Text style={styles.errorText}>{errorMsg}</Text>
      </View>
    );
  }

  if (!prayerTimes) {
    return (
      <View style={styles.container}>
        <ActivityIndicator size="large" />
        <Text>Fetching Prayer Times...</Text>
      </View>
    );
  }

  return (
    <View style={styles.container}>
      <Text style={styles.header}>Prayer Times for Today</Text>
      {Object.entries(prayerTimes).map(([name, time]) => (
        <View key={name} style={styles.timeRow}>
          <Text style={styles.prayerName}>{name}</Text>
          <Text style={styles.prayerTime}>{time}</Text>
        </View>
      ))}
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 20, justifyContent: 'center' },
  header: { fontSize: 24, fontWeight: 'bold', marginBottom: 20, textAlign: 'center' },
  timeRow: { flexDirection: 'row', justifyContent: 'space-between', marginVertical: 8 },
  prayerName: { fontSize: 18 },
  prayerTime: { fontSize: 18, fontWeight: 'bold' },
  errorText: { color: 'red', fontSize: 16, textAlign: 'center' },
});
```

---

*Summary:*  
- Requests location permission.  
- Gets current location.  
- Calculates prayer times for today using `islamic-prayer-times` package.Phase 5: Daily Verse Reminders

---

*1. Setup local notifications (Expo example):*

```bash
expo install expo-notifications
```

---

*2. Create `/utils/notifications.js`*

```javascript
import * as Notifications from 'expo-notifications';
import * as Permissions from 'expo-permissions';

export async function scheduleDailyVerseNotification(verse) {
  const { status } = await Notifications.requestPermissionsAsync();
  if (status !== 'granted') return;

  await Notifications.cancelAllScheduledNotificationsAsync();

  // Schedule notification daily at 9 AM
  await Notifications.scheduleNotificationAsync({
    content: {
      title: "Daily Quran Verse",
      body: verse,
    },
    trigger: {
      hour: 9,
      minute: 0,
      repeats: true,
    },
  });
}
```

---

*3. Example usage in `/screens/DailyVerseScreen.js`*

```javascript
import React, { useEffect } from 'react';
import { View, Text, Button, StyleSheet } from 'react-native';
import { scheduleDailyVerseNotification } from '../utils/notifications';

const exampleVerse = "Indeed, with hardship comes ease. (Quran 94:6)";

export default function DailyVerseScreen() {
  useEffect(() => {
    scheduleDailyVerseNotification(exampleVerse);
  }, []);

  return (
    <View style={styles.container}><Text style={styles.verse}>{exampleVerse}</Text>
      <Button title="Reschedule Notification" onPress={() => scheduleDailyVerseNotification(exampleVerse)} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: 'center', alignItems: 'center', padding: 20 },
  verse: { fontSize: 20, textAlign: 'center', marginBottom: 20 },
});
```

---

*Summary:*  
- Requests notification permission.  
- Schedules daily notification at a fixed time.  
- Displays sample verse and button to reschedule.

---

Ready for Phase 6?Phase 6: Hadith Library (Bukhari, Muslim, etc.)

---

*1. Setup local JSON data file `/data/hadiths.json`*  
Example structure:

```json
[
  {
    "id": 1,
    "book": "Bukhari",
    "hadith": "Actions are judged by intentions.",
    "reference": "Bukhari, 1"
  },
  {
    "id": 2,
    "book": "Muslim",
    "hadith": "The best among you are those who learn the Quran and teach it.",
    "reference": "Muslim, 1"
  }
]
```

---

*2. Create `/screens/HadithLibraryScreen.js`*

```javascript
import React, { useState, useEffect } from 'react';
import { View, Text, FlatList, StyleSheet, TouchableOpacity } from 'react-native';
import hadiths from '../data/hadiths.json';

export default function HadithLibraryScreen() {
  const [filteredHadiths, setFilteredHadiths] = useState(hadiths);
  const [selectedBook, setSelectedBook] = useState('All');

  const books = ['All', 'Bukhari', 'Muslim'];

  const filterHadiths = (book) => {
    setSelectedBook(book);
    if (book === 'All') {
      setFilteredHadiths(hadiths);
    } else {
      setFilteredHadiths(hadiths.filter(h => h.book === book));
    }
  };

  return (
    <View style={styles.container}>
      <View style={styles.filterContainer}>
        {books.map(book => (
          <TouchableOpacity
            key={book}style={[styles.filterButton, selectedBook === book && styles.selectedFilter]}
            onPress={() => filterHadiths(book)}
          >
            <Text style={styles.filterText}>{book}</Text>
          </TouchableOpacity>
        ))}
      </View>
      <FlatList
        data={filteredHadiths}
        keyExtractor={item => item.id.toString()}
        renderItem={({ item }) => (
          <View style={styles.hadithItem}>
            <Text style={styles.hadithText}>"{item.hadith}"</Text>
            <Text style={styles.referenceText}>— {item.book}, {item.reference}</Text>
          </View>
        )}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 20 },
  filterContainer: { flexDirection: 'row', marginBottom: 10, justifyContent: 'space-around' },
  filterButton: { padding: 10, borderRadius: 20, backgroundColor: '#eee' },
  selectedFilter: { backgroundColor: '#4287f5' },
  filterText: { 
