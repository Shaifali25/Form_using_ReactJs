import React,{useState} from 'react';

function App(){

    const[entry,setEntry]= useState({
        fname:"",
        lname:"",
        email:"",
        num:"",
        pass:"",


    });

    const [errors, setErrors] = useState({
        fname: "",
        lname: "",
        email: "",
        num: "",
        pass: "",
      });
    
    
    const handleChange = (event) => {
        const { name, value } = event.target;
    
        setEntry((prevEntry) => ({
          ...prevEntry,
          [name]: value,
        }));
    
        // Call validation function on change
        validateField(name, value);
      };
    
      const validateField = (name, value) => {
        let error = "";
    
        switch (name) {
          case "fname":
          case "lname":
            if (value.trim() === "") {
              error = "This field is required";
            } else if (!/^[A-Za-z]+$/.test(value)) {
              error = "Only letters are allowed";
            }
            break;
          case "email":
            if (value.trim() === "") {
              error = "Email is required";
            } else if (!/\S+@\S+\.\S+/.test(value)) {
              error = "Invalid email format";
            }
            break;
          case "num":
            if (value.trim() === "") {
              error = "Number is required";
            } else if (!/^\d+$/.test(value)) {
              error = "Only digits are allowed";
            } else if (value.length !== 10) {
              error = "Number should be 10 digits";
            }
            break;
          case "pass":
            if (value.trim() === "") {
              error = "Password is required";
            } else if (value.length < 6) {
              error = "Password must be at least 6 characters";
            }
            break;
          default:
            break;
        }
    
        setErrors((prevErrors) => ({
          ...prevErrors,
          [name]: error,
        }));
      };
    
      const handleSubmit = (event) => {
        event.preventDefault();
    
        // Check if all fields are valid before submission
        let isValid = true;
        Object.keys(entry).forEach((key) => {
          validateField(key, entry[key]);
          if (errors[key] !== "") {
            isValid = false;
          }
        });
    
        if (isValid) {
            const formattedData = {
                first_name: entry.fname,
                last_name: entry.lname,
                phone_number: entry.num,
                email: entry.email,
                password: entry.pass,
              };
          console.log("Form Submitted: ", entry);
        } else {
          console.log("Validation Errors: ", errors);
        }
      };

    
    return(
     <div className='container'>
        <h1>Fill it</h1>
         <form onSubmit={handleSubmit}>
            <input 
            name="fname" 
            type="text" 
            placeholder="First Name"  
            value={entry.fname}
            onChange={handleChange}

          />
          <div>{errors.fname && <p>{errors.fname}</p>}</div>
            <input 
            name="lname" 
            type="text" 
            placeholder="Last Name" 
            value={entry.lname}
            onChange={handleChange}

            />
            <div>{errors.lname && <p>{errors.lname}</p>}</div>

            <input 
            name="email" 
            type="email" 
            placeholder="Email" 
            value={entry.email}
            onChange={handleChange}
            />
            <div>{errors.email && <p>{errors.email}</p>}</div>

            <input 
            name="num" 
            type="number" 
            placeholder="Enter your number" 
            value={entry.num}
            onChange={handleChange}

            />
            <div>{errors.num && <p>{errors.num}</p>}</div>
           
            <input 
            name="pass" 
            type="password" 
            placeholder="Enter your password" 
            value={entry.pass}
            onChange={handleChange}
            />

            <div>{errors.pass && <p>{errors.pass}</p>}</div>

            <button type="submit">Submit</button>
         </form>
     </div>
    );
}

export default App;