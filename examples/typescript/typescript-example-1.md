```ts
    let mdoContents = `
# Example Keyed

- Id: 10
- Hash: 9b87da04349185080af34e04895d4d66
- Properties:
    - Length:
        - Type: number
        - Enabled: false
        - Value: -1
    - Description:
        - Type: string
        - Enabled: true
        - Value: example`;
    
    let mdo = MDOL.parse(mdoContents, MDOL.camelCase);
    console.log(mdo.id); // outputs 10
    console.log(mdo.hash); // outputs "9b87da04349185080af34e04895d4d66"
    console.log(mdo.properties.length); // outputs { type: "number", enabled: false, value: -1 }
```
