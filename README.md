# EmployeeApi
This is the api for CallingApi.

This is an API application. The app is providing API for CRUD tasks.

Index.js is all about connecting with database and calling routes.

//Dabase ->
mongoose.connect(process.env.DB_CONNECTION, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
    useFindAndModify: false,
    useCreateIndex: true
}, () => {});

//Homepage for apies ->
app.get('/', (req, res) => {
    res.send('Welcome to the Employees Api');
});

//Route ->
const employeeRoute = require("./routes/Employees")
app.use('/employee', employeeRoute);


Model define the schema for the Database.

const EmployeeSchema = mongoose.Schema({
    name: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true
    },
    dateOfBirth: {
        type: String,
        required: true
    },
    phoneNumber: {
        type: Number,
        required: true
    },
    salary: {
        type: Number,
        required: true
    },
    department: {
        type: String,
        required: true
    },
});

Route is the route for Api so you perform CRUD.

//Finding all Employee ->
router.get('/', async(req, res) => {
    try {
        const employees = await Employee.find();
        res.json(employees);
    } catch (err) {
        res.json({
            message: err
        });
    }
});

//Submitting Employee ->
router.post('/', async(req, res) => {
    const employee = new Employee({
        name:req.body.name,
        email:req.body.email,
        dateOfBirth:req.body.dateOfBirth,
        phoneNumber:req.body.phoneNumber,
        salary:req.body.salary,
        department: req.body.department      
    });
    try {
        await employee.save();
        res.json(employee);
    } catch (err) {
        res.json({
            message: err
        });
    }
});

//Delete Employee ->
router.delete('/:EmployeeId', async(req, res) => {
    try {
        await Employee.deleteOne({ _id: req.params.EmployeeId });
        res.send("OK")
    } catch (err) {
        res.json({
            message: err
        });
    }
});

//update a Employee ->
router.patch('/:EmployeeId', async(req, res) => {
    try {
        await Employee.updateOne({ _id: req.params.EmployeeId }, { $set: { title: req.body.title } });
        res.send("OK")
    } catch (err) {
        res.json({
            message: err
        });
    }
});


