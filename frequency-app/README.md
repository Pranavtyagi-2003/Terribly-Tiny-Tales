
# Frequency App

This is a react app where you will get a histogram of words frequency and "export to csv" option after fetching text file.

[Live website link](https://gentle-hummingbird-5c06b9.netlify.app/)

* This website is hosted on netlify.


## Installation

Install and run frequency-app with npm-vite

```bash
npm create vite@latest
cd my-project
npm install
npm run dev 
```

Install the recharts npm package for histogram

```bash
npm install recharts
```
Install the axios npm package for fetching text file
```bash
npm install axios
```


    
## Documentation

1. Create a folder named components in src folder.
2. Create files named FileFetch.jsx , FileFetch.css in components folder.
3. Create a functional component in FileFetch.jsx.
4. import axios and some child components from recharts.

```bash
import axios from "axios";
import {
  ResponsiveContainer,
  BarChart,
  Bar,
  XAxis,
  YAxis,
  Tooltip,
  Label,
  LabelList,
} from "recharts";
```
# Count frequencies 
1. Call the function calculateLetterFrequencies on onclick with passing text file from submit button.

```bash
<button onClick={() =>calculateWordFrequencies
("https://www.terriblytinytales.com/test.txt")}
  className="submit">
   Submit
</button>
```
2. calculateWordFrequencies function 
```bash
const calculateWordFrequencies = async (link) => {
    const response = await axios.get(link);
    const text = response.data.toLowerCase();
    const words = text.split(/\s+/);
    const frequencies = {};

    for (let i = 0; i < words.length; i++) {
      const word = words[i];
      if (frequencies[word] === undefined) {
        frequencies[word] = 1;
      } else {
        frequencies[word]++;
      }
    }

    const sortedFrequencies = Object.entries(frequencies)
      .sort((a, b) => b[1] - a[1])
      .slice(0, 20);
    const sortedObject = Object.fromEntries(sortedFrequencies);
    setWordFrequencies(sortedObject);
  };
 ```
# Explaination of calculateWordFrequencies function  

1. Fetch the file using axios, change it to toLowerCase and split it using split function.
```bash
 const response = await axios.get(link);
 const text = response.data.toLowerCase();
 const words = text.split(/\s+/);
 ```   
2. Iterate the text and find frequencies of each word and store it in array of Objects named frequencies.
```bash
   const frequencies = {};
    for (let i = 0; i < words.length; i++) {
      const word = words[i];
      if (frequencies[word] === undefined) {
        frequencies[word] = 1;
      } else {
        frequencies[word]++;
      }
    }
```  
3. Sort the array of Objects in descending order using sort function and slice top 20 most occurring words of text file.AFter that change it to fromEntries and set it to useState array of Object. 
```bash
const sortedFrequencies = Object.entries(frequencies).sort((a, b) => b[1] - a[1]).slice(0, 20);
const sortedObject = Object.fromEntries(sortedFrequencies);
setWordFrequencies(sortedObject);  
``` 
# Store data with proper key : value format
1. Iterate the array of object using forEach loop and store it using key with "letter" and value with "frequency".
```bash
let barDataArray = [];
Object.keys(wordFrequencies).forEach((key) => {
    words.push(key);
    const value = wordFrequencies[key];
    freq.push(value);
    const newData = { letter: key, frequency: value };
    barDataArray.push(newData);
  });
```
2. Example :-
```bash
 barDataArray = [
   {
     letter : "can",
     frequency : "10"
   },
   {
     letter : "do",
     frequency : "5"
   },
   ]
```   

# Export to cvs
1. Call the function handleExportClick from export button.
```bash
<button onClick={handleExportClick} className="export"> Export</button>
```    
2. Function handleExportClick   
```bash
function handleExportClick() {
    let csvContent = "data:text/csv;charset=utf-8,";
    csvContent += words + "\n";
    csvContent += freq + "\n";
    const encodedUri = encodeURI(csvContent);
    const link = document.createElement("a");
    link.setAttribute("href", encodedUri);
    link.setAttribute("download", "data.csv");
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
  }
```
# Explaination of handleExportClick function 
1. Here, we create the csvContent variable and set it to "data:text/csv;charset=utf-8,", which is the format for a data URI.
```bash
let csvContent = "data:text/csv;charset=utf-8,";
```
2. Next, we add the header row and data row to the csvContent variable.
```bash
csvContent += words + "\n";
csvContent += freq + "\n";
```
Here words and freq are the arrays of words and frequency.

3. Finally, we create a data URI with the csvContent variable, create a link element with the href set to the data URI and the download attribute set to "data.csv", append the link to the DOM, click the link, and remove the link from the DOM.
```bash
const encodedUri = encodeURI(csvContent);
const link = document.createElement("a");
link.setAttribute("href", encodedUri);
link.setAttribute("download", "data.csv");
document.body.appendChild(link);
link.click();
document.body.removeChild(link);
```
# Histogram Creation
1. Import the ResponsiveContainer, BarChart, Bar, XAxis, YAxis, Tooltip, Label, LabelList from recharts.
```bash
import {
  ResponsiveContainer,
  BarChart,
  Bar,
  XAxis,
  YAxis,
  Tooltip,
  Label,
  LabelList,
} from "recharts";
```
2. Create a container of <ResponsiveContainer> then inside <BarChart>. Assign the data to <XAxis>, <YAxis> and create a Bars using <Bar>.<Tooltip> for showing data on hover. <Label> for showing XAxis and YAxis information.<LabelList> for showing details on top of bars.

3. Create a bar graph using these components.
```bash
<ResponsiveContainer width="90%" aspect={3}>
                <BarChart data={barDataArray} width={400} height={400}>
                  <XAxis dataKey="letter">
                    <Label
                      value="20 Most Occurring Letters"
                      offset={-2}
                      position="insideBottom"
                    />
                  </XAxis>
                  <YAxis dataKey="frequency">
                    <Label
                      value="Frequency Of Letters"
                      angle={-90}
                      position="left"
                      offset={-1}
                      textAnchor="middle"
                    />
                  </YAxis>
                  <Tooltip />
                  <Bar dataKey="frequency" fill="red">
                    <LabelList dataKey="frequency" position="top" />
                  </Bar>
                </BarChart>
</ResponsiveContainer>
```
# Reset the data
1. Call the function reset from button Reset
```bash
<button className="reset" onClick={reset}>Reset<button>
```
2. In function reset we are making array empty. 
```bash
function reset() {
    freq.length = 0;
    setWordFrequencies({});
  }
```
# Apply the CSS
1. Import the FileFetch.css in FileFetch.jsx.
```bash
import "./FetchFile.css";
```
2. CSS file code.
```bash
.main-div{
    height: 100vh;
}
.graph{
    height: 80vh;
}
.buttons{
    height: 20vh;
    display: flex;
    justify-content: space-evenly;
    align-items: center;
}
button{
    height: 5vh;
}

.para{
    text-align: center;
}

.submit{
    width: 6%;
    height: 4.5vh;
    background-color: blue;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: .2rem;
    font-size: 15px;
}
.submit:hover{
    background-color: rgb(37, 37, 147);
}

.export{
    width: 8%;
    height: 4.5vh;
    background-color: green;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: .2rem;
    font-size: 15px;
}
.export:hover{
    background-color: rgb(8, 81, 8);
}

.reset{
    width: 6%;
    height: 4.5vh;
    background-color: red;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: .2rem;
    font-size: 15px;
}
.reset:hover{
    background-color: rgb(162, 10, 10);
}
.graph{
    margin-top: 4rem;
    margin-left: 7rem;
}

@media (max-width:768px) {
    .buttons{
        width: 100%;
        flex-direction: column;
    }
    .submit{
        width: 25%;
    }
    .reset{
        width: 25%;
    }
    .export{
        width: 25%;
    }
    .graph{
        width: 150%;
        margin-left: 0;
        height: 200vh;
        margin-top: 6rem;
    }
}
```

# Thankyou happy hacking !!!!











