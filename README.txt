Steps Creating MERN Api :	


1-  cd /server     then
        npm install "sequelize": "^6.33.0", "sequelize-cli": "^6.2.0"
  
2-   inside './server' -->  sequelize init .... it will create new files for mysql.
        contains --> models folder  + index.                      

3- install mysql DB.
4- install mysql workbench.
5- connect the data base name inside './server/config/json'.


-------------------------------------------------

1- Connected nodejs express server.
2- imported './server/models' inside server.jor named here './server/index.js' - 
    inside it we connected the db Schema for mysql to the express listen server 3001.

3- now the go './server/routes/Posts.js'    -  export it into the server.js .



--------------------------------------------------------------------------------------------------

The Front-END : 

1- cd "./clienet"   --> npx create-react-app "Here the name of the folder".

2- npm i axios .---> axios is connecting to the DB using nodejs inside the Home.js

3- Open 2 terminals one running the server and the other the client react.

4- npm i cors .--> import cors into the "./server/index"
--------------------------------------------------------------------------------------------------
// connecting the server with the DB & displaying the Pages :


1- cd client --> npm i react-router-dom

2- in the client a folder named pages -- contains all the routes.

3- Home.js

4- In the App.js --> using <Link> to the pages routing.

5- npm i formik  --> in the client.

6- npm i yup  --> import it inside CreatePost.js

--------------------------------------------------------------------------------------------------



1- getting the id when pressing of the div. --> inside HOME.JS  .
2- useNavigate .


--------------------------------------------------------------------------------------------------
redirecting the page after posting :

1- inside  CreatePost.js :
        IMPORT createHistory.... which is useNavigate now . it will navigate to the home page after posting.

--------------------------------------------------------------------------------------------------
1-  creating new model for the comment in DB.
2- create the commentsroute.js.
3- create the comments router inside the app.js or index.js :
        const commentsRouter = require("./routes/Comments");
        app.use("/comments", postRouter);
 
4- import the commentsmodel into commentsrouter.js  .




--------------------------------------------------------------------------------------------------

Connecting the COMMENT with the POST : 

1- inside the Post model :
          Posts.associate = (models) => {
            Posts.hasMany(models.Comments, {
            onDelete: "cascade",
         });
      };

2- Create Comment Route.


3- In the Comment get route : 
          const comments = await Comments.findAll({ where: { PostId: postId } });


4- insid the "./client/posts.js" --> 
        when navigating inside the Post we call 2 axios links, the post int he left and map through all
        the comments in the rights and displaying it.



5- When adding a new comment we want to appear dynamically , so in the addComment function : 
        .then((response) => {
        console.log("Comment added!");
        const commentToAdd = { commentBody: newComment };
        setComments([...comments, commentToAdd]);
      });



--------------------------------------------------------------------------------------------------
Creating the USers : 

1- create Users.js model.
2- in the model create the association that each user has multiple posts.
3- Create the Users.js int routers folder.

4- inside the index.js 
        const usersRouter = require("./routes/Users");
        app.use("/auth", usersRouter);

5- npm i bcrypt --> for hashing and saving the hashed pass in to the DB.

6- go to the Users.js in the router -> create registeration route.

--------------------------------------------------------------------------------------------------
JSON Web Token : 

- here will store it in our session storage :

1- in the server folder - npm i jsonwebtoken
2- require it in the User.js route file --> const { sign } = require("jsonwebtoken");
3- Create the token after the user loggin 

Then : 
4- In the login.js Component in the client.
        if (response.data.error) alert(response.data.error);
        //   else
        sessionStorage.setItem("accessToken", response.data);

5- Checking Session storage in google chrome :Go to Application inside there is a - KEY & Value.


--------------------------------------------------------------------------------------------------
 Adding for the Comments an owner name :
 1- Create in the server a folder named middlewares and  add a a file named AuthMiddleware.js

 2- import the AuthMiddleware.js into the comments.js Route.

 3- Go to the clint Pages Post.js - here we are creating the comment.

// Try to add a times of post for each comment.



--------------------------------------------------------------------------------------------------
Displaying the username per each COMMENT : 

1- Add the username column in the Comment DB table.
2- in the AuthMiddleware.js file -->   req.user = validToken;
3- then in the comment route file --> 
         // for Showing the username per each posted comment :
            const username = req.user.username; // copy the username from the authenticated user.
            comment.username = username;

4- Now go to the client Post.js that displaying the comments.

now the comments will be added into the DB.

5- dynamically show the username as the comment posted.
          const commentToAdd = {
            commentBody: newComment,
            username: response.data.username,
          };


--------------------------------------------------------------------------------------------------

Render Login the registeration links when there is no token : 

1- in the App.js client.

         {/* If there is no variable in the sessionStorage called accessToken then display if there is the display the other */}
         {!sessionStorage.getItem("accessToken") && (
            <>
              <Link to="/login">Login</Link>
              <Link to="/registration">Registeration</Link>
            </>
          )}


2- change sessionStorage to localStorage

3- localStorage will help when closing the window and opening it it will 
   still logged in.

4- Rerender the page after logging in to change in the page.
        - Now the above coding of hiding the logging & registeration links will not hide.
        - A) Create helpers folder in the "client/src/helpers"
                AuthContext.js
                import { createContext } from "react";
                export const AuthContext = createContext("");

        - B) In the App.js which is the highest level import './helpers'
        - C) Wrap it around any Component that you want to access that context.
                      <AuthContext.Provider>
        - D) useState in the Login.js Client will set setAuthState to TRUE after successful login.


5- If we refresh the page it will render the login and registeration links again....
                - use the useEffect in this situation in the App.js client.



6- Preventing the acceptance of fake Tokens like :
        -localStorage.getItem('accessToken','asdasdasdasdas');

        A) In the server/users.js route  file --> create auth route. 
                router.get("/auth", ((((validateToken))), (req, res) => {
                res.json(req.user);
                });


--------------------------------------------------------------------------------------------------
logging OUT :



--------------------------------------------------------------------------------------------------

Deleting a Comment : 

1- in the comments.js route.

2- in the posts.js --> add buttong for each button.
        A) import the AuthContext.js
        B)   const deleteComment = (id) => {
    axios
      .delete(`http://localhost:3001/comments/${id}`, {
        headers: { accessToken: localStorage.getItem("accessToken") },
      })
      .then(() => {
        setComments(
          comments.filter((val) => {
            // only keep the comment that has the val.id if its not equal the id passed in the main function parameter.
            return val.id != id;
          })
        );
      });
  };



        C)    {authState.username === comment.username && (
                  <button
                    onClick={() => {
                      deleteComment(comment.id);
                    }}
                  >
                    X
                  </button>





--------------------------------------------------------------------------------------------------

Creating the Like Button : 

1- Create a new model DB call it Likes.js .
2- inside the post.js model add this :
           Posts.hasMany(models.Likes, {
      onDelete: "cascade",
    });

3- inside the Users model add this :
           Users.hasMany(models.Likes, {
      onDelete: "cascade",
    });


4- Create a Likes.js route file  

5- in the index file ./server/index.js   add this 
                // #4 USERS :
const likesRouter = require("./routes/Likes");
app.use("/like", likesRouter);


6-  Testing in POSTMAN  --> add the Token in the HEADERs to be as logged in user.

7- Only one LIKE per post --> 

8 Adding the Like BTN in the frontEnd --> in the Home.js Page  we gonna add the like functionality.

9- Showing the number of like beside the post --> Go to the Post.js Route.
        - join the like and post tables and unt how many IDs.
        - so inside the post.js route -->    const listOfPosts = await Posts.findAll({ include: [Likes] });

        - Then Go to the client Home.js  -->  <label>{value.Likes.length}</label>

        _ and add this to likeAPost function :
           // Update an object inside a state : grabbing the list and modefying it and we setting the state of the list to equal the modefied list.
        setListOfPosts(
          listOfPosts.map((post) => {
            if (post.id === postId) {
              // Keep the post elements as it is & change the Likes field & the Kep the likes as it is and Add a ZERO at the end of this array.
              return { ...post, Likes: [...post.Likes, 0] };
            } else {
              return post;
            }
          })
        );

        - now its updating but in an increasing way not addidn the deleting So we gonna go to teh Like.js Route :



--------------------------------------------------------------------------------------------------

Likes icon : need a review : 

1- in the client --> install from material-ui.com
        - npm install @mui/material @emotion/react @emotion/styled
        -  Go to the Home.js in the CLient.
        - from the website copy the needed import and import it and then use it like this.
        - import ThumbUpIcon from "@mui/icons-material/ThumbUp";
        -  <ThumbUpIcon
                onClick={() => {
                  likeAPost(value.id);
                }}
              />


--------------------------------------------------------------------------------------------------
Fixing Bugs : 

1- Locking the posts route for the unlogged un users.
         // First Check if the user loggedin - if not redirect him into the login page if he is logged in show the home page for him.
    if (!authState.status) {
      navigate("/login");
    }



2- redirect the unlogged in user from createpost route to the log in page.

3- Hide the hoome & creaate a post links from the unlogged in users.
        - in the App.js


4- removing the input username field from the create a post link - it supposed to know automatically.
        - Go to the create post client folder.
        -  



--------------------------------------------------------------------------------------------------
Adding Profile Page: 

1- create profile ,js 
2- add its link inside App.js
3- create its route inside Users.js Router. -- it will be in the index as same user route './auth/'

4- Query for profile info in Client Profile.js page using axios.

5- when getting the profile info we want to know all the posted from the info user. 
6- Go inside the Posts.js route and 


--------------------------------------------------------------------------------------------------
Edit and Updating a post : 

1- insid post.js Component.


                const editPost = (option) => {
                if (option === "title") {
                  // Prompt :
                  let newTitle = prompt("Enter new title: ");
                } else {
                  let newTitle = prompt("Enter new Text : ");
                }
              };

                className="title"
                        onClick={() => {
                          editPost("title");
                        }}
              className="body"
                        onClick={() => {
                          editPost("body");
                        }}


2- Go to the Post.js Route  : 


                  router.put("/title", validateToken, async (req, res) => {
            // 1) Grab the title from the body :
            const { newTitle, id } = req.body;

            // 2) Update the title column & pass the value of title which we got from the Req.Body - where id is equal to id :
            await Posts.update({ title: newTitle }, { where: { id: id } });

            res.send(newTitle);
          });

          router.put("/postText", validateToken, async (req, res) => {
            // 1) Grab the title from the body :
            const { newText, id } = req.body;

            // 2) Update the title column & pass the value of title which we got from the Req.Body - where id is equal to id :
            await Posts.update({ postText: newText }, { where: { id: id } });

            res.send(newText);
          });


3- Go to Post.sj clienet  .


        onClick={() => {
                      if (authState.username === postObject.username) editPost("title");
                      axios.put(
                        `http://localhost:3001/posts/title`,
                        {
                          newTitle: newTitle,
                          id: id,
                        },
                        {
                          headers: {
                            accessToken: localStorage.getItem("accessToken"),
                          },
                        }
                      );
                    }}
        onClick={() => {
                      if (authState.username === postObject.username) editPost("body");
                      axios.put(
                        `http://localhost:3001/posts/postText`,
                        {
                          newText: newPostText,
                          id: id,
                        },
                        {
                          headers: {
                            accessToken: localStorage.getItem("accessToken"),
                          },
                        }
                      );
                    }}



4- Automatically Updating the UI post updated without refreshing.




--------------------------------------------------------------------------------------------------

Updating the Password : 

1- Go to Profile.js page -- Add BTN
       -  <button>Change My Password</button>
       - its going to appear in every profile even for the not signed in user : to prevent it :
       A) import { AuthContext } from "../helpers/AuthContext";
          const { authState } = useContext(AuthContext);
       
       B)  
        {authState.username === username && <button>Change My Password</button>}


2- Create i Pages Client ChangePassword.js file :

3- Add the file inside the App.js to be identified as a route :
            <Route path="/changepassword" exact Component={ChangePassword} />



4- Adde the routing to the BTN :
          {authState.username === username && (
                  <button
                    onClick={() => {
                      navigate("/changepassword");
                    }}
                  >
                    Change My Password
                  </button>
                )}


5- in changepassword.js :

        const ChangePassword = () => {
        return (
          <div>
            <h1>Change Your Password</h1>
            <input type="text" placeholder="Old Password..."></input>
            <input type="text" placeholder="New Password..."></input>
            <button>Save Ch anges.</button>
          </div>
        );
      };



6- Inside Users.js Route

            // validateToken will get the username :
            router.put("/changepassword", validateToken, async (req, res) => {
              // 1) Grab the password from the body :
              const { oldPassword, newPassword } = req.body;

              // 2) Check if the old password is matched in the system : current === old :
              const user = await Users.findOne({ where: { username: req.body.username } });

              bcrypt.compare(oldPassword, user.password).then(async (match) => {
                if (!match) res.json({ error: "Wrong Password Entered!" });

                // 3 if the match is perfect : Hash the password the new password :
                bcrypt.hash(password, 10).then(async (hash) => {
                  // 3) Save the original username & hashed password to the DB :
                  await Users.update(
                    { password: hash },
                    { where: { username: req.user.username } }
                  );

                  // 4) Send the response :
                  res.json("SUCCESS");
                });
              });
            });




 

--------------------------------------------------------------------------------------------------

Deploying the Application :

1-  in GitHub - make a repo fro the BackENd and FrontENd.
2- Heroku - 
2- Using Render Hosting - 
3-    .gitignore node_module/ package-lock.json

4- 

      git init
      server git:(master) ✗ git branch -M main
      ➜  server git:(main) ✗ git add .
      ➜  server git:(main) ✗ git commit -m "Initial Commit"
      ➜  server git:(main) git remote add origin https://github.com/Basseloob/Full-Stack-Client-Perdo-Render-Hosting.git
      ➜  server git:(main) git push -u origin main


5- Go Render website :
A) new webservice - connect to GitHub - Only select Repo - 