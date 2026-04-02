```ts
    let molContents = `
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
    
    let mol = MDOL.parse(molContents, MDOL.camelCase);
    console.log(mol.id); // outputs 10
    console.log(mol.hash); // outputs "9b87da04349185080af34e04895d4d66"
    console.log(mol.properties.length); // outputs { type: "number", enabled: false, value: -1 }
```
