const express = require('express');
const fs = require('fs');
const path = require('path');

// Initialize the Express application
const app = express();
const PORT = process.env.PORT || 3001;

// Middleware for parsing JSON and urlencoded form data
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Static middleware to serve the public folder
app.use(express.static('public'));

// GET route for notes page
app.get('/notes', (req, res) => {
  res.sendFile(path.join(__dirname, 'public', 'notes.html'));
});

// GET route for fetching all notes
app.get('/api/notes', (req, res) => {
  fs.readFile(path.join(__dirname, 'db', 'db.json'), 'utf8', (err, data) => {
    if (err) throw err;
    res.json(JSON.parse(data));
  });
});

// POST route for saving a new note
app.post('/api/notes', (req, res) => {
  const newNote = req.body;
  fs.readFile(path.join(__dirname, 'db', 'db.json'), 'utf8', (err, data) => {
    if (err) throw err;
    const notes = JSON.parse(data);
    newNote.id = notes.length ? notes[notes.length - 1].id + 1 : 1; // Assign an id to the new note
    notes.push(newNote);
    fs.writeFile(
      path.join(__dirname, 'db', 'db.json'),
      JSON.stringify(notes),
      (err) => {
        if (err) throw err;
        res.json(newNote);
      }
    );
  });
});

// DELETE route for deleting a note by id
app.delete('/api/notes/:id', (req, res) => {
  const idToDelete = parseInt(req.params.id);
  fs.readFile(path.join(__dirname, 'db', 'db.json'), 'utf8', (err, data) => {
    if (err) throw err;
    let notes = JSON.parse(data);
    notes = notes.filter((note) => note.id !== idToDelete);
    fs.writeFile(
      path.join(__dirname, 'db', 'db.json'),
      JSON.stringify(notes),
      (err) => {
        if (err) throw err;
        res.json({ success: true });
      }
    );
  });
});

// Wildcard route to serve index.html for any route not explicitly defined
app.get('*', (req, res) => {
  res.sendFile(path.join(__dirname, 'public', 'index.html'));
});

// Start the server
app.listen(PORT, () => {
  console.log(`Server is listening on port ${PORT}`);
});
