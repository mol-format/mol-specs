```ts
    let molContents = `
        # User 10
        
        Id: 10
        Username: janedoe
        Full name: Jane Doe
        
        ## Password
        
        Hash: aoX5r5pS5b3YF2z7LyN1g2dJ7pZ7s2P4G8H8Q2a1A
        Algorithm: argon2id
        Version: 19`;
    
    let user = MOL.parse(molContents, MOL.camelCase);
    console.log(user.id); // outputs 10
    console.log(user.username); // outputs "janedoe"
    console.log(user.fullName); // outputs "Jane Doe"
    console.log(user.password); // outputs { hash: "aoX5r5pS5b3YF2z7LyN1g2dJ7pZ7s2P4G8H8Q2a1A", algorithm: "argon2id", Version: 19 }
```
