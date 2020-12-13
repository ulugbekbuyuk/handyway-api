# handyway-api
Just sample api


OrderObject
{
    id: "MongoDB ObjectID (string) ",
    code: Number (6 digit unique number for each order),
    status: Number, (hozircha 0 yoki 1 bulishi mumkin, 0 - bu qabul qilindi, 1 - junatildi, kiyin 2, 3, 4 lar ham qushiladi),
    comment: String (some optional comment about this order from Customer),
    createdAt: Date, (* pastda tushintirish beraman)
    updatedAt: Date, (* pastda tushintirish beraman)
    items: [
        {
            title: String, (product title bu, misol: Coca-Cola 1.5 L),
            code: Number, (product code i)
            price: Number,
            quantity: Number
        },
        {
            title: String, (product title bu, misol: Coca-Cola 1.5 L),
            code: Number, (product code i)
            price: Number,
            quantity: Number
        }
    ]
}

* createdAt, updatedAt lar avtomatiski qushiladi, agar mongo Schema pastdagidaqa busa,
const OrderSchema = ({
    status: Number,
    comment: String,
    ...
}, { timestamps: true });

***  { timestamps: true } shuni qushib quyselar uzi avtomatik createdAt, updatedAt ni har bir Order dokumentga qushib quyadi




----------------------------------------------------------------
// Admin & Customer
/orders     GET
Request.Header = {
    token: "JWT.TOKEN"
}
Response.Body = [{OrderObject}, {OrderObject}, {OrderObject}, ]

*** Logika shunaqaki, agar Admin surov qisa hamma Order lar status i "0" bulganlarini junatadi,
*** agar Customer surov qisa, faqat shu Customer ga tegishli Orderlar status i "0" bulganlarini
*** JWT Tokenda quyidagi malumotlar buladi:
{
    title: "Admin" admin uchun yoki "Customer.Title",
    role: "admin" Yoki "customer"
    id: "null" for Admin, "Customer.id" for Customers
}
*** Manashu JWT dagi malumotlar adashmasam, req.identifier ni ichida buladi, bu authorize middleware tayyorlab beradi
*** hullas "role" va "id" ga qarab kim admin-u kim qaysi customer ekanini bilib osa buladi.
----------------------------------------------------------------

// Admin & Customer
/orders?status=0     GET
Yoki
/orders?status=1     GET
Vahakazo ***?status=2,   ****?status=3
Request.Header = {
    token: "JWT.TOKEN"
}
Response.Body = [{OrderObject}, {OrderObject}, {OrderObject}, ]

*** Tepadagi Logika bilan bir hil
----------------------------------------------------------------

// Only Admin
/orders/:id             DELETE
Request.Header = {
    token: "ADMIN.JWT.TOKEN"
}
Response.Body = {OrderObject}

----------------------------------------------------------------

// Only Admin
/orders/:id             PUT
Request.Header = {
    token: "ADMIN.JWT.TOKEN"
}
Request.Body = {
    status: 1
}
Response.Body = {OrderObject}

*** Admin faqat Orderni status qisminigina yangilay oladi holos

----------------------------------------------------------------

// Only Customer
/orders            POST
Request.Header = {
    token: "JWT.TOKEN"
}
Request.Body = {
    OrderObject
}
Response.Body = {OrderObject}       *** javobda OrderObject ga "id" qushilib qoladi

----------------------------------------------------------------
----------------------------------------------------------------
----------------------------------------------------------------
