# Test-Code
var express = require("express");
var app = express();
var request = require("request");
var bodyparser = require("body-parser");
var bitcore = require("bitcore-lib");

app.use(bodyparser.urlencoded({
    extended: true
}));
app.use(bodyparser.json());

app.get("/", function(req, res){
    res.sendFile(__dirname + "/index.html");
});

app.post("/wallet", function(req, res){
    var brainsrc = req.body.brainsrc;
    console.log(brainsrc);
    var input = new Buffer(brainsrc);
    var hash = bitcore.crypto.Hash.sha256(input);
    var bn = bitcore.crypto.BN.fromBuffer(hash);
    var pk = new bitcore.Privatekey(bn).towWIF();
    var addy = new bitcore.Privatekey(bn).toAddress();
    res.send("The Brain Wallet of: " + brainsrc); 
});

app.listen(80, function(){
    
    console.log("go");
});
