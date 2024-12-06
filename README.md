Certainly! Below is a complete **frontend implementation** for the **MERN stack YouTube clone** using **React** and **Styled Components**. The frontend fetches video data from the backend API (`/api/videos`), displays the videos in styled cards, and uses a basic layout with a global theme.

### **Frontend Setup:**

#### 1. **Install Dependencies**

First, you need to install the necessary dependencies for React, Styled Components, and Axios:

```bash
npx create-react-app client
cd client
npm install styled-components axios react-router-dom
```

#### 2. **Create Folder Structure**

Here’s the folder structure for the React frontend:

```
/client
  /src
    /components
      VideoCard.js
    /pages
      Home.js
    /styles
      GlobalStyle.js
    App.js
    index.js
```

#### 3. **Global Styles (GlobalStyle.js)**

This file defines the global styles for your application.

```jsx
// src/styles/GlobalStyle.js
import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  body {
    font-family: 'Arial', sans-serif;
    background-color: #f4f4f4;
    color: #333;
    line-height: 1.6;
  }

  a {
    text-decoration: none;
    color: inherit;
  }
`;

export default GlobalStyle;
```

#### 4. **Styled Button Component (Button.js)**

Let’s create a styled button component for possible future use.

```jsx
// src/components/Button.js
import styled from 'styled-components';

const Button = styled.button`
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

export default Button;
```

#### 5. **Video Card Component (VideoCard.js)**

This component is used to display each video card.

```jsx
// src/components/VideoCard.js
import React from 'react';
import styled from 'styled-components';

const Card = styled.div`
  width: 300px;
  margin: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  background-color: #fff;
  cursor: pointer;
  transition: transform 0.2s;

  &:hover {
    transform: scale(1.05);
  }
`;

const VideoThumbnail = styled.img`
  width: 100%;
  height: auto;
`;

const VideoTitle = styled.h3`
  font-size: 1.2rem;
  margin: 10px;
  padding: 0 10px;
  color: #333;
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

#### 6. **Home Page (Home.js)**

This page is where we fetch and display the video data.

```jsx
// src/pages/Home.js
import React, { useEffect, useState } from 'react';
import axios from 'axios';
import VideoCard from '../components/VideoCard';
import styled from 'styled-components';

const Container = styled.div`
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 20px;
`;

const LoadingText = styled.div`
  font-size: 1.5rem;
  color: #555;
  margin-top: 20px;
`;

const Home = () => {
  const [videos, setVideos] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchVideos = async () => {
      try {
        const response = await axios.get('http://localhost:5000/api/videos');
        setVideos(response.data);
        setLoading(false);
      } catch (error) {
        console.error('Error fetching videos:', error);
        setLoading(false);
      }
    };

    fetchVideos();
  }, []);

  if (loading) {
    return <LoadingText>Loading videos...</LoadingText>;
  }

  return (
    <Container>
      {videos.length === 0 ? (
        <LoadingText>No videos available</LoadingText>
      ) : (
        videos.map((video) => <VideoCard key={video._id} video={video} />)
      )}
    </Container>
  );
};

export default Home;
```

#### 7. **App Component (App.js)**

This file ties everything together and includes routing.

```jsx
// src/App.js
import React from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import GlobalStyle from './styles/GlobalStyle';
import Home from './pages/Home';

const App = () => {
  return (
    <Router>
      <GlobalStyle />
      <Routes>
        <Route path="/" element={<Home />} />
      </Routes>
    </Router>
  );
};

export default App;
```

#### 8. **Main Entry (index.js)**

Finally, the entry point for your React application, where React renders the app.

```jsx
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

const rootElement = document.getElementById('root');
const root = ReactDOM.createRoot(rootElement);

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

---

### **Running the Application**

1. **Start the React app**:

```bash
cd client
npm start
```

This will run the frontend locally on `http://localhost:3000`.

2. **Start the backend server** (assuming you have the backend set up on port `5000`):

```bash
cd server
node index.js
```

This makes your API available at `http://localhost:5000/api/videos`.

---

### **What This Includes:**
- **Global Styles**: A global style is applied to the app using `createGlobalStyle` from `styled-components`.
- **Video Cards**: Each video is displayed in a styled card that includes the thumbnail and title.
- **Fetching Data**: Video data is fetched from the backend and displayed in the home page.
- **Styled Components**: All components (like the video card, button, and layout) are styled using `styled-components`.
- **Responsive Layout**: The `Container` in the `Home` page uses `flex-wrap` to make the layout responsive.

### **Next Steps:**
- **Expand the UI**: Add more components, such as a video player, comments section, and a search bar.
- **Enhance Features**: Implement features like user authentication, likes/dislikes, comments, etc.
- **Styling**: Customize and improve the design further with more advanced `styled-components`.

This should give you a solid foundation for your MERN stack YouTube clone with React and Styled Components.
