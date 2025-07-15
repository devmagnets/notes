# MAP
>use this
```
{x.map(item,index)=>(
    <p key={index}>{item.title}</p>
)} 
```
>avoid return in map
```
{x.map(item,index)=>{
    return(<>
   <p key={index}>{item.title}</p> 
    </>)
}
}
```

# Responsive Navbar
- render at mobile screen first and hide it on md on mobile screen maintain usestate to open menubar and close menu bar 
- again render at md for desktop and hide it at mobile
> we are using navlink from react-router-dom it help to navigate to different page and provide active class to tell user on which page they are by styling isactive is a boolean 

```
<NavLink
        className={item.title != 'Home' && (({ isActive }) => isActive ? "border-b-2 border-text-primary text-yellow-100" : "hover:text-yellow-100")}
        to={item.link}
        >
        About us
</NavLink>
```

> browser route is used to use navlink and route to different page on client withour refreshing the page
```
 <BrowserRouter>
    <Routes>
      <Route path='/' element={<Home />} />
    </Routes>
  </BrowserRouter>
```

## Example-Responsive Navbar With Routing Code Logic Complete
```
## App.jsx

import Navbar from './components/Navbar.jsx'
import Footer from './components/Footer.jsx'
import Home from './pages/Home.jsx'
import Blog from './pages/Blog.jsx'
import Gallery from './pages/Gallery.jsx'
import Process from './pages/Process.jsx'
import Project from './pages/Project.jsx'
import { BrowserRouter, Route, Routes } from 'react-router-dom'
export default function App() {

return (<>
  <BrowserRouter>
    <Navbar />
    <Routes>
      <Route path='/' element={<Home />} />
      <Route path='/process' element= {<Process />}/>
      <Route path='/projects' element= {<Project/>} />
      <Route path='/blogs' element= {<Blog />}/>
      <Route path='/gallery' element= {<Gallery/>  }/>
    </Routes>
    <Footer />
  </BrowserRouter>
</>)
}
```

```
## Navbar.jsx
import { MdOutlineClose, MdOutlineMenu } from "react-icons/md"; {/*this is menubar and close menu bar icon in svg format*/}
import { useState } from 'react';
import { NavLink } from "react-router-dom";
export default function Navbar() {
    const [isMenuClicked, setMenuClicked] = useState(false)
    const handleMenuBar = () => {
        setMenuClicked(!isMenuClicked);
    }
    const navData = [
        { title: 'Home', link: '/' },
        { title: 'Process', link: '/process' },
        { title: 'Projects', link: '/projects' },
        { title: 'Gallery', link: '/gallery' },
        { title: 'Blogs', link: '/blogs' },
    ]
    return (<>
        <nav className=" px-10 flex sticky top-0 w-full z-10 bg-primary text-text-primary  justify-between h-12 items-center text-base">
            {/* text color  and bg and px-10 */}
            <h1>Dilip Construction</h1>
            <ul className="hidden md:flex gap-4 ">
                {navData.map((item, index) => (

                    <li key={item.title}>
                        <NavLink
                            className={item.title != 'Home' && (({ isActive }) =>
                                isActive ? "border-b-2 border-text-primary text-yellow-100" : "hover:text-yellow-100")}
                            to={item.link}>
                            {item.title}
                        </NavLink>
                    </li>

                ))}
            </ul>
            <div className="md:hidden" onClick={handleMenuBar}>
                {isMenuClicked ? <MdOutlineClose /> : <MdOutlineMenu />}
            </div>

            {isMenuClicked && (
                <ul className="left-0 absolute top-12 w-full gap-8 py-5 bg-gray-800 text-text-primary backdrop-blur-[7px] flex flex-col items-center">
                    {navData.map((item, index) => {
                        return (<>
                            <li onClick={handleMenuBar} key={index}><NavLink to={item.link}>{item.title}</NavLink></li>
                        </>)
                    })}
                </ul>
            )}
        </nav>
    </>)

}
```



# Form
- button tag is by default submit we can use it we can change its type to make normal click button by type='button' 
- disabled={true} will disable button used to avoid sending multiple submission together by default while one is being sent 
- label tag has htmlfor which match id due to which even we are clicking on text of label input box get active to write with 
   corresponding input matched with label htmlfor id
```
 <form onSubmit={sendMail} action="">
    <label htmlFor="name">Name</label>
    <input placeholder="Enter your Name" type="text" name='name' id='name' onChange={handleChange} />
    <input disabled={true} type="submit" value="Submit Details" />
 </form>
```
# Contact Form With Logic
- we want to send object by converting into json to parse object in backend therfore to store object value in realtime we use usestate
- we take empty object with name and value is empty 
- we make this object as usestate variable
- using handlechange function we set form data
- e.target to get name and value it is reserved keyword
- prev function to set only changed data not whole

```
  const formStructure = { name: '', email: '', phoneNo: '', message: '' }
    const [formData, setFormData] = useState(formStructure)

    const handleChange = (e) => {
        const { name, value } = e.target
        setFormData((prev) => {
            return { ...prev, [name]: value }
        })
    }
```
# Cors
> allow to send frontend data to backend and get data from backend to frontend
```
const cors=require('cors')
app.use(cors()) {/*it is cors() not cors*/}
```
# Data Parsing

> Frontend object → JSON.stringify() → 🔁 → Express.json() → Backend object
```
const data = {
  name: "Abhishek",
  email: "abc@example.com",
  message: "Hello",
};

fetch('/api/contact', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(data),  // 🔁 Convert JS object to JSON string
});

app.use(express.json())

req.body.name      // "Abhishek"
req.body.email     // "abc@example.com"
req.body.message   // "Hello"

while fetching data we use  const data = await response.json(); to transform json to object
```
#  Example-Sending Form Data From Frontend To Backend
```
    const [sendingStatus, setSendingStatus] = useState(false)
    const sendMail = async (e) => {
        setSendingStatus(true)
        e.preventDefault();

        try {
            const response = await fetch('http://localhost:2030/mail/sent', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify(formData)
            });

            if (!response.ok) {
                // Handle non-200 errors
                const errorText = await response.text();
                console.error("Server error:", errorText);
                alert("Something went wrong on the server.");
                return;
            }

            const data = await response.json();
            console.log("Response:", data);
            alert(data.message || "Mail sent successfully!");
        } catch (error) {
            console.error("Fetch error:", error);
            alert("Failed to send. Please check the server connection or console for details.");
        }
        finally {
            setSendingStatus(false); // Re-enable button
        }
    };


                <form onSubmit={sendMail} className="rounded-2xl px-5 py-5 text-gray-300 bg-primary  w-full md:w-[30%] h-[80%] flex flex-col gap-[12px]" action="">
                    <label htmlFor="name">Name</label>
                    <input placeholder="Enter your Name" type="text" name='name' id='name' className="bg-primary border-[1px] px-3 rounded-lg h-8" onChange={handleChange} />
                    <label htmlFor="email">Email *</label>
                    <input placeholder='Enter your Email' type="email" name="email" id='email' required className="bg-primary border-[1px] px-3 rounded-lg h-8" onChange={handleChange} />
                    <label htmlFor="phoneNo">Phone No. *</label>
                    <input placeholder='Enter Phone No' type="tel" name="phoneNo" id='phoneNo' required className="bg-primary border-[1px] px-3 rounded-lg h-8 " onChange={handleChange} />
                    <label htmlFor="message">Message *</label>
                    <textarea name="message" id="message" placeholder='Message' required className="bg-primary border-[1px] px-3 rounded-lg h-16 resize-none " onChange={handleChange} ></textarea>
                    <button disabled={sendingStatus} className=' text-white ml-auto mr-auto w-[80%] bg-text-primary hover:bg-text-primary/20 border-black/45 border-2 px-7 py-2 rounded-2xl' > {sendingStatus ? "Sending..." : "Submit"}</button>
                </form>
```
# Mail Backend

```
       const transporter = nodemailer.createTransport({
            service: 'gmail',
            auth: {
                user: 'devmagnets.mail@gmail.com', {/*main account of gamil you used to send message*/}
                pass: process.env.GMAIL_PASSWORD {/*create password in google > app password*/}
            }
        })
        const mailOption = {
            from: 'devmagnets.mail@gmail.com', {/*same as user */}
            to: 'singhabhishek.engineer@gmail.com',{/*you choose to get client email*/}
            replyTo: 'xyz@gmail.com,{/*client email where you want to sent reply*/}
            subject: 'inquiry',
            html: <p>hi<p>
        }
        transporter.sendMail(mailOption, (err, info) => {
            if (err) {
                console.log(err.message)
            }
            else {
                console.log(info.response)
            }
        })
```
#  Example-Mail With Logic Backend
- we use <%= x %> to change its variable i.e x, according to request
- we use ejsrenderFile to get html as template to use any where
>    const htmlContent = await ejs.renderFile('./view/emailTemplate.ejs', {
            name: name, email: email, phoneNo: phoneNo, message: message
        });

```
#emailTemplate.ejs


                    <tr>
                        <td>
                            <strong>Name: </strong> <%= name %> 
                        </td>

                    </tr>
                    <tr>
                        <td style="word-break: break-all;">
                            <strong>Email: </strong><%= email %>
                        </td>

                    </tr>
                    <tr>
                        <td>
                            <strong>Phone: </strong><%= phoneNo %>
                        </td>
                    </tr>
#email.js 

const express = require('express')
const router = express.Router()
const ejs = require('ejs')
const nodemailer = require('nodemailer')

router.post('/sent', async (req, res) => {
    const { name, email, phoneNo, message } = req.body
    try {
        const htmlContent = await ejs.renderFile('./view/emailTemplate.ejs', {
            name: name, email: email, phoneNo: phoneNo, message: message
        });
        const transporter = nodemailer.createTransport({
            service: 'gmail',
            auth: {
                user: 'devmagnets.mail@gmail.com',
                pass: process.env.GMAIL_PASSWORD
            }
        })
        const mailOption = {
            from: 'devmagnets.mail@gmail.com',
            to: 'singhabhishek.engineer@gmail.com',
            replyTo: email,
            subject: 'inquiry',
            html: htmlContent
        }
        transporter.sendMail(mailOption, (err, info) => {
            if (err) {
                console.log(err.message)
                res.send({ message: 'failed' })
            }
            else {
                console.log(info.response)
                res.send({ message: 'sent' })
            }
        })
    } catch (err) {
        console.log(err.message)
        res.send('failed try catch')

    }
})

module.exports = router
```

# Separate data content from jsx file 
- we use export or export default like we do in jsx file it is same
```
#processData.js


const step = {
  title: 'Step Process',
  list: ['Consultation', 'Design'],
};

const extra = {
  title: 'Extra Info',
  list: ['Durability', 'Aesthetics'],
};

export { step, extra }; 
{/* 
export default step 
export {extra}
*}

-----------------------------------------------------------------------
#process.jsx


import { step, extra } from '../pagesData/processData';

console.log(step.title);  // Output: 'Step Process'
console.log(extra.list);  // Output: ['Durability', 'Aesthetics']

```
# Separating Route
- using module.export we export routing to use in index.js
- we first import 
- then using use middleware pass it with path i.e app.use('/user', userRoutes) if user request user send to userRoutes means 
if user sent request to user/contact then we will see contact from submitted look below to understand basically both path routing and index both are concatenated
```
#index.js

const userRoutes = require('./routes/userRoutes'); // importing routes
app.use('/user', userRoutes); // prefix all routes with /user

---------------------------------------------------------------------

#routes/userRoutes.js

router.post('/contact', (req, res) => {
  res.send('Contact Form Submitted');
});

module.exports = router;
```

# Example-Separating Route

```
#routes/userRoutes.js


const express = require('express');
const router = express.Router();

// Example route
router.get('/about', (req, res) => {
  res.send('About Page');
});

router.post('/contact', (req, res) => {
  res.send('Contact Form Submitted');
});

module.exports = router;

-----------------------------------------------------------

#index.js


const express = require('express');
const app = express();
const userRoutes = require('./routes/userRoutes'); // importing routes

app.use('/user', userRoutes); // prefix all routes with /user

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```