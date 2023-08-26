# React Handling Loading States

```jsx
/**
 * ------------------------
 * HANDLING LOADING STATES
 * ------------------------
 * When fetching data from an externam API, the date copuld be in one of a few different states
 * 1st is loading state, just loading hasnt come back yet
 * 2nd is success state which you have data to display or an error state
 *
 * Any sort of asynchronous request we need to handle a loading state while waiting for data to come back
 * Need to handle success case as well as error state
 *
 * These three can be represented using useState Hooks
 */

// Another component
function GithubUser({ name, location, avatar }) {
  return (
    <div className="GithubUser">
      <h1>{name}</h1>
      <p>{location}</p>
      <img
        src={avatar}
        alt={name}
        style={{ height: "150px", borderRadius: "50%" }}
      />
    </div>
  );
}

import "./App.css";
import { useState, useEffect } from "react";

// useState to handle the data
// useEffect to make the call for the external data
function App() {
  let url = "https://api.github.com/users/m-soro";

  // create container for the data
  const [data, setData] = useState(null);
  // adding these two states
  // create a value for error
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(false);

  // time to represent the above three to the component

  // call the api
  useEffect(() => {
    setLoading(true);
    // fetch is built into the browser. A way of making http request to get data from a source
    // once we get the data take that response and call .json() to turn it into json
    fetch(url)
      .then((response) => response.json())
      // then passs this json data to our data container
      .then(setData)
      // set the setLoading to false since we have the data already
      .then(() => setLoading(false))
      // if there si some sort of error set an Error
      .catch(setError);
    // pass an empty array to tell useEffect to only get data once
  }, []);

  // show a message if loading
  if (loading) return <h1>Loading...</h1>;
  // if error of any kind, return a pre-formatted json string to display
  if (error) return <pre>{JSON.stringify(error)}</pre>;
  // if there is no data return null
  if (!data) return null;

  // display the github user component if everything goes as expected
  return (
    <GithubUser
      name={data.name}
      location={data.location}
      avatar={data.avatar_url}
    />
  );
}

export default App;
```
