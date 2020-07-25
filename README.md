## Steps

To run server: npm run server

Make a server file hook up express. Bring in packages morgan, colors, dotenv, express.

Make a config folder and file for global variables. Use dotenv to access these values in server.js

Setup PORT and listen and display a message if the server is running. Make and check the first route with Postman

Make a routes folder and make a tranactions.js file within. Bring in express and router. Make a request to test `router.get('/', (req, res) => res.send('Hello'));` and bring it into server.js.

Make a folder called controllers at route. Make a file transactions.js (it can also be called transactionsController.js). All the methods will be stored in this file for transactions to interact with the database. Write a getTransactions function to test
`exports.getTransactions = (req, res, next) => { res.send('GET Transactions'); };`

Bring it into the transactions.js in controllers. This is how to do it with router `router.route('/').get(getTransactions);`

Add two more functions to transactions controller addTransaction and deleteTransaction. It's a post and delete request. Test them in postman

Make a mongoDb cluster with AWS. Connect the application by clicking on connect in the cluster.https://www.youtube.com/watch?v=KyWaXA_NvT0&t=119s Make a name for the database and cluster, you will need a database user and password which you will need to paste into the connection string later. Click connect your application. Grab the connection string and put it in the config file under MONGO_URI

Make a file called db.js in the config folder. Bring in mongoose. make an async function called connectDB with a try catch within. need to use mongoose.connect() and within is the environmental varibale for MONGO_URL. Console log a good message if good. Else console log error and then process.exit(1); Bring the connectDB function into server.js and call it. ( after the config and before the routes)

Create a models folder with a Transaction.js file. Bring in monogoose. Create a schema for the model called TransactionSchema = new mongoose.Schema({}) . The schema goes inside the brackets. Add a text , amount and createdAt schema. This is how to export this `module.exports = mongoose.model('Transaction', TransactionSchema);`

Bring the new schema model into the controllers file. Make the three functions async. Make a try catch in getTransactions. store this in a variable await Transaction.find(). return a sucess status with these three keys success count data. In the error send a 500 with success and error key. Check in postman. The transactions should be empty.

To add a transaction need to do a middleware parser in server.js to access the req body. Add this after you initiate express `app.use(express.json());` Back in the controller file destrcture text and amount from the req.body. Store in a variable Transaction.create(req.body). Remember this is a promise in an async function. send a 201 status with success and the transaction as the data. Do this in a try and can test by sending a JSON transaction in Postman
_the catch is tricky_
make an if else in the catch. Can send an empty post request in postman and console.log(err) to see the validation message. if (err.name === 'ValidationError'). Inside collection the errors messages in an array using Object.values and mapping over each of these values and getting the message. Send a status 400( which is bad client data )and send success false and error as the messages array just made (can test this again in Postman). ELSE send the same error stuff from the getTransactions function.

To delete a transaction do a findById. Inside the try if not a transaction return a 404 else do an remove on the transaction and do success message. in the catch handle error same as other way in above functions. You can grab an id from an item in the db put the id in the params in postman and delete.

in client package JSON add proxy.
`"proxy": "http://localhost:5000"`
in package json at the root at this to scripts.
`"dev": "concurrently \"npm run server\" \"npm run client\""`
Add axios in the client folder. cd into client and
`npm i axios`
cd .. and then
`npm run dev`

Integrate with the backend
Make initialState transactions a blank array.
Create a new action called getTransactions in GlobalState.js. It should be async because it is an axios request (need to bring axios in). Need to dispatch type 'GET_TRANSACTIONS' and set the response from the backend as the payload. Add to initial state an error property set to null and a loading property. For the error in the getTransactions dispatch dispatch a type called 'TRANSACTION_ERROR' and the payload will be the error response.

Add the dispatch types to the reducer. For GET_TRANSACTIONS (make first case), get the state set the loading to false and use the payload to set transactions. For the 'TRANSACTION_ERROR' (last case), get state and the error is action.payload.

Pass the getTransaction, error and loading function to provider. Call the getTransactions from the transactionList. Put it in a useEffect hook. If you want to see requests in the terminal can add this to server.js underneath the middleware body parser.
`if ((process.env.NODE_ENV = 'development')) { app.use(morgan('dev')); }`

Add an \_ to the id in transaction.js and in the dispatcher to match the mongoose ID for deleteTransaction. Make delete transaction async. Make an await axios request to delete transaction and dispatch as before. In catch do as done in getTransactions function.

In addTransaction in dispatch. Make async. Since posting data need a variable object config. This is headers `headers: { 'Content-Type': 'application/json' }`. In the try do an axios post. Need for parameter 1. endpoint 2. the data 3. the config. For the dispatch payload it needs to be the response from this request. Handle error in the same way as above.

You can add format number function if you want to handle numbers with commas. Can add util folder in src and a format.js file.

If you want to make it ready for production cd into the client folder. and run `npm run build`. cd back into the server folder. Add path to server file. `const path = require('path');` . Beneath the routes in server.js and above the port include this code.

```
if (process.env.NODE_ENV === 'production') {
app.use(express.static('client/build'));

app.get('\*', (req, res) => res.sendFile(path.resolve(\_\_dirname, 'client', 'build', 'index.html')));
}
```

You will also need to go into the config and change from development to production. Run npm start at root of server.
