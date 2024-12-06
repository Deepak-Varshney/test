Here’s a simple setup for a **MERN stack** YouTube clone with **React**, **Styled Components**, and **Node.js** (Express) as an example. Below are code snippets for both the **React frontend** and **Node.js backend**:

---

### **Frontend (React + Styled Components)**

1. **Install Dependencies**

```bash
npm install react-router-dom styled-components axios
```

2. **Example Component: Video Card**

```jsx
// src/components/VideoCard.js
import React from 'react';
import styled from 'styled-components';

const Card = styled.div`
  width: 300px;
  margin: 10px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  overflow: hidden;
`;

const VideoThumbnail = styled.img`
  width: 100%;
  height: auto;
`;

const VideoTitle = styled.h3`
  font-size: 1.2rem;
  margin: 10px;
`;

const VideoCard = ({ video }) => {
  return (
    <Card>
      <VideoThumbnail src={video.thumbnail} alt={video.title} />
      <VideoTitle>{video.title}</VideoTitle>
    </Card>
  );
};

export default VideoCard;
```

3. **App Component (Fetching Data)**

```jsx
// src/App.js
import React, { useEffect, useState } from 'react';
import styled from 'styled-components';
import axios from 'axios';
import VideoCard from './components/VideoCard';

const Container = styled.div`
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
`;

const App = () => {
  const [videos, setVideos] = useState([]);

  useEffect(() => {
    const fetchVideos = async () => {
      const result = await axios.get('http://localhost:5000/api/videos');
      setVideos(result.data);
    };

    fetchVideos();
  }, []);

  return (
    <Container>
      {videos.map((video) => (
        <VideoCard key={video._id} video={video} />
      ))}
    </Container>
  );
};

export default App;
```

4. **Styled Components Example for Theming**

```jsx
// src/theme.js
import { createGlobalStyle } from 'styled-components';

export const GlobalStyle = createGlobalStyle`
  body {
    font-family: 'Arial', sans-serif;
    background-color: #f4f4f4;
    color: #333;
    margin: 0;
    padding: 0;
  }
`;

export const Button = styled.button`
  background-color: ${(props) => (props.primary ? '#007BFF' : '#f1f1f1')};
  color: ${(props) => (props.primary ? 'white' : '#333')};
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;

  &:hover {
    background-color: ${(props) => (props.primary ? '#0056b3' : '#ddd')};
  }
`;
```

---

### **Backend (Node.js + Express + MongoDB)**

1. **Install Dependencies**

```bash
npm install express mongoose cors
```

2. **Server Setup (index.js)**

```js
// server/index.js
const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');

const app = express();
const port = 5000;

// Middleware
app.use(cors());
app.use(express.json());

// Connect to MongoDB
mongoose.connect('mongodb://localhost/youtube-clone', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('MongoDB connected'))
  .catch(err => console.log(err));

// Video Schema
const videoSchema = new mongoose.Schema({
  title: String,
  thumbnail: String,
  description: String
});

const Video = mongoose.model('Video', videoSchema);

// Routes
app.get('/api/videos', async (req, res) => {
  try {
    const videos = await Video.find();
    res.json(videos);
  } catch (err) {
    res.status(500).send('Server Error');
  }
});

// Start server
app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
```

3. **Adding Sample Data (MongoDB)**

```js
// Use this in your MongoDB shell or via a script to insert sample videos:
const mongoose = require('mongoose');
const Video = require('./models/Video'); // Assuming the Video model is in 'models/Video.js'

mongoose.connect('mongodb://localhost/youtube-clone', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => {
    const sampleVideos = [
      { title: 'Sample Video 1', thumbnail: 'https://via.placeholder.com/300', description: 'A sample video' },
      { title: 'Sample Video 2', thumbnail: 'https://via.placeholder.com/300', description: 'Another sample video' },
    ];
    return Video.insertMany(sampleVideos);
  })
  .then(() => console.log('Sample videos added'))
  .catch(err => console.error(err));
```

---

### **Folder Structure**
```
/client
  /src
    /components
      VideoCard.js
    App.js
    theme.js
  package.json
/server
  index.js
  /models
    Video.js
  package.json
```

---

### **Running the Application**

1. **Start the backend server**:

```bash
cd server
node index.js
```

2. **Start the React frontend**:

```bash
cd client
npm start
```

---

### **Summary**
- The frontend (React) uses **Styled Components** to style individual components like `VideoCard`.
- The backend (Node.js + Express) serves data from a MongoDB database.
- This example fetches video data from the backend and displays it using styled components in the frontend.
