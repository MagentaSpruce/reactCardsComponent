# reactToursApp
This React app fetches an array of objects from an API and populates them into cards advertising travel tours.

Constructing this project helped me to better learn and practice the following:
1) useState()
2) UseEffect()
3) Conditional rendering

A brief overview of the pertinent React code is given below:
 
 First the data is fetched from the API and formatted correctly. Depending on if data is being retrieved or not their will be a loading screen displayed. To monitor and set this loading screen state is used.
 ```React
   const fetchTours = async () => {
     const [loading, setLoading] = useState(true);
    setLoading(true);
    try {
      const response = await fetch(url);
      const tours = await response.json();
      setLoading(false);
      setTours(tours);
    } catch (error) {
      setLoading(false);
      console.log(error);
    }
  };
  ```
  
  
  Another state is added to the retreived tour data which starts out as an empty array. 
  ```React
   const [tours, setTours] = useState([]);
   ```
   
   
   UseEffect() is called to run the fetchTours() function directly after the pages initial loading and render conditionally.
   ```React
     useEffect(() => {
    fetchTours();
  }, []);
  if (loading) {
    return (
      <main>
        <Loading />
      </main>
    );
  }
  if (tours.length === 0) {
    return (
      <main>
        <div className="title">
          <h2>no tours left</h2>
          <button className="btn" onClick={fetchTours}>
            refresh
          </button>
        </div>
      </main>
    );
  }
  return (
    <main>
      <Tours tours={tours} removeTour={removeTour} />
    </main>
  );
  ```
  
  
  The prop set inside of <Tours /> is used to pass the values of the data objects to the Tour.jsx file for use there where it is deconstructed and used to populate part of the page. The readMore state is added to monitor the paragraph text and render only 200 characters unless the relevant button is clicked.
  ```React
  const Tour = ({ id, image, info, price, name, removeTour }) => {
  const [readMore, setReadMore] = useState(false);
  return (
    <article className="single-tour">
      <img src={image} alt={name} />
      <footer>
        <div className="tour-info">
          <h4>{name}</h4>
          <h4 className="tour-price">${price}</h4>
        </div>
        <p>
          {readMore ? info : `${info.substring(0, 200)}`}...
          <button onClick={() => setReadMore(!readMore)}>
            {readMore ? "show less" : "show more"}
          </button>
        </p>
        <button
          className="delete-btn"
          onClick={() => {
            removeTour(id);
          }}
        >
          Not Interested
        </button>
      </footer>
    </article>
  );
};
```


The tours were mapped over and changed, including the setting of an ID which is required for the delete functionality.
```React
const Tours = ({ tours, removeTour }) => {
  return (
    <section>
      <div className="title">
        <h2>Our Tours</h2>
        <div className="underline"></div>
      </div>
      <div>
        {tours.map((tour) => {
          return <Tour key={tour.id} {...tour} removeTour={removeTour} />;
        })}
      </div>
    </section>
  );
};
```


Finally the removeTour() functionality was added inside of the App() function.
```React
  const removeTour = (id) => {
    const newTours = tours.filter((tour) => tour.id !== id);
    setTours(newTours);
  };
  ```
  
  
 ***End walkthrough
