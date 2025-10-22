# BugDB v1 
- Platform: Hacker101
- Category: Web, GraphQL 
- Difficulty: Easy
# Micro-CMS v1 
- Platform: Hacker101
- Category: Web 
- Difficulty: Easy
  
  ## Flag0
  Hints:
  - What can you see? What can you not see?
  - What data types are involved?
  - Have you tried querying different endpoints?

<img width="1451" height="940" alt="image" src="https://github.com/user-attachments/assets/54f9b854-76ec-47eb-8440-f75c7f41d661" />
<img width="1454" height="513" alt="image" src="https://github.com/user-attachments/assets/27481494-94d7-45b5-b6e6-d1652f215a72" />
<img width="1283" height="672" alt="image" src="https://github.com/user-attachments/assets/fa3f6ece-18bc-4ef9-a48a-ed8e28e1c4e1" />
<img width="1257" height="477" alt="image" src="https://github.com/user-attachments/assets/13c04dd3-ee0b-4903-a5f5-32aafdfc6315" />
<img width="1454" height="606" alt="image" src="https://github.com/user-attachments/assets/21aebbdc-8588-4154-8741-2c43885c7b73" />

<img width="1454" height="503" alt="image" src="https://github.com/user-attachments/assets/e9c5607d-c63d-46b7-a52e-16b1340df88b" />
<img width="1454" height="511" alt="image" src="https://github.com/user-attachments/assets/eb890d59-9e1c-4f31-b0cc-6d24fe7975ce" />
<img width="975" height="554" alt="image" src="https://github.com/user-attachments/assets/2e14829d-de8f-4b6f-b45d-135d97d77ca7" /><br><br>

The visual summary of the nesting: <br>
```
Query
 └─ user (UsersConnection)
      ├─ pageInfo (PageInfo)
      │    ├─ startCursor
      │    └─ endCursor
      └─ edges (UsersEdge)
           ├─ cursor
           └─ node (Users)
                ├─ id
                ├─ username
                └─ bugs (Bugs_Connection)
                     ├─ pageInfo (PageInfo)
                     │    ├─ startCursor
                     │    └─ endCursor
                     └─ edges (Bugs_Edge)
                          ├─ cursor
                          └─ node (Bugs_)
                               ├─ id
                               ├─ reporterId
                               ├─ text
                               ├─ private
                               └─ reporter (Users)
                                    └─ id
```

This information builds the final query that reveals the flag. <br>

```
query{
  user {
    edges {
      node {
        id
        username
        bugs {
          pageInfo {
            startCursor
            endCursor
          }
          edges {
            cursor
            node {
              id
              reporterId
              text
              private
              reporter {
                id
              }
            }
          }
        }
      }
    }
  }
}
```

<img width="1680" height="781" alt="image" src="https://github.com/user-attachments/assets/4e6a76b9-4cb5-48cd-97df-0fe6904997a2" />

 
  
