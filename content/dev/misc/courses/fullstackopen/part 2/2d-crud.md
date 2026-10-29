# REST Conventions \& CRUD Operations with json-server

## 1. Resources \& URLs

1. Definition \& Use Case

- **Resources:** Data objects (e.g., notes) identified by URLs.
- **Use Case:** `GET /notes` returns all notes; `GET /notes/3` returns note with `id=3`.

2. Syntax
```http
GET    /notes         → List all notes  
GET    /notes/:id     → Get single note  
POST   /notes         → Create new note  
PUT    /notes/:id     → Replace entire note  
PATCH  /notes/:id     → Modify part of note  
DELETE /notes/:id     → Remove note  
```

3. Example
```bash
curl http://localhost:3001/notes/3  
```


***

## 2. Creating (POST)

1. Definition \& Use Case

- **Definition:** POST to collection URL creates a new resource.
- **Use Case:** Send JSON in request body; server assigns `id`.

2. Syntax
```js
axios.post('/notes', { content, important })
  .then(res => setNotes(notes.concat(res.data)))
```

3. Example
```js
const addNote = e => {
  e.preventDefault()
  const newNote = { content, important: Math.random() < 0.5 }
  axios.post('http://localhost:3001/notes', newNote)
    .then(res => {
      setNotes(notes.concat(res.data))
      setContent('')
    })
}
```


***

## 3. Updating (PUT \& PATCH)

1. Definition \& Use Case

- **PUT:** Replace entire resource; send full object.
- **PATCH:** Modify specific fields; send partial object.

2. Syntax
```js
// Toggle importance with PUT
const toggle = id => {
  const note = notes.find(n => n.id===id)
  const updated = { ...note, important: !note.important }
  axios.put(`/notes/${id}`, updated)
    .then(res => setNotes(notes.map(n => n.id===id ? res.data : n)))
}

// Partial update with PATCH
axios.patch(`/notes/${id}`, { important: true })
  .then(res => …)
```

3. Example
```js
const toggleImportance = id => {
  const note = notes.find(n => n.id===id)
  const changed = { ...note, important: !note.important }
  noteService.update(id, changed)
    .then(updatedNote => {
      setNotes(notes.map(n => n.id===id ? updatedNote : n))
    })
    .catch(() => {
      alert(`Note '${note.content}' was removed`)
      setNotes(notes.filter(n => n.id!==id))
    })
}
```


***

## 4. Deleting (DELETE)

1. Definition \& Use Case

- **Definition:** DELETE removes a resource by its URL.
- **Use Case:** Confirmation prompt; remove from state on success.

2. Syntax
```js
if (window.confirm(`Delete ${person.name}?`)) {
  axios.delete(`/persons/${id}`)
    .then(() => setPersons(persons.filter(p=>p.id!==id)))
}
```

3. Example
```js
const deletePerson = id => {
  const person = persons.find(p=>p.id===id)
  if (window.confirm(`Delete ${person.name}?`)) {
    personService.remove(id)
      .then(() => {
        setPersons(persons.filter(p=>p.id!==id))
      })
      .catch(() => {
        alert('Already removed')
        setPersons(persons.filter(p=>p.id!==id))
      })
  }
}
```


***

## 5. Full CRUD in Phonebook

```js
// service module: services/persons.js
import axios from 'axios'
const baseUrl = '/persons'
export const getAll    = () => axios.get(baseUrl).then(r=>r.data)
export const create    = newObj => axios.post(baseUrl,newObj).then(r=>r.data)
export const update    = (id,obj) => axios.put(`${baseUrl}/${id}`,obj).then(r=>r.data)
export const remove    = id => axios.delete(`${baseUrl}/${id}`)


// App.jsx
useEffect(() => {
  personService.getAll().then(setPersons)
}, [])

const addPerson = e => {
  e.preventDefault()
  if (persons.some(p=>p.name===newName)) {
    if (window.confirm(`Replace number for ${newName}?`)) {
      const existing = persons.find(p=>p.name===newName)
      personService.update(existing.id,{ ...existing, number:newNumber })
        .then(updated => {
          setPersons(persons.map(p=>p.id===updated.id?updated:p))
        })
    }
  } else {
    personService.create({ name:newName, number:newNumber })
      .then(p => setPersons(persons.concat(p)))
  }
}

const deletePerson = id => { … } // as above

return (
  <>…<Persons persons={displayed} onDelete={deletePerson}/>…</>
)
```


***

**Key Takeaways**

- **POST** to `/collection` creates;
- **PUT** (full) / **PATCH** (partial) to `/collection/:id` updates;
- **DELETE** to `/collection/:id` removes;
- Always use `key` in list rendering;
- Encapsulate CRUD in service modules.
<span style="display:none">[^1]</span>
