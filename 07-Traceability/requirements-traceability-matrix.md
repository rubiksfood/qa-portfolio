# Requirements Traceability Matrix

Project: Shopping List App

| Req ID | Requirement                                        | Test Level                | Test Reference                                     |
|--------|----------------------------------------------------|---------------------------|----------------------------------------------------|
| R1     | User can register                                  | Backend + E2E             | auth.test.js, auth.spec.ts                         |
| R2     | User can log in                                    | Backend + Frontend + E2E  | auth.test.js, Login.test.jsx, auth.spec.ts         |
| R3     | Only authenticated users can access /shopping-list | Backend + Frontend + E2E  | middleware.test.js, routing.test.jsx, auth.spec.ts |
| R4     | User can create item                               | Backend + Frontend + E2E  | shopItem.test.js, ShopList.test.jsx, shop.spec.ts  |
| R5     | Users cannot access other users’ items             | Backend + E2E             | shopItem.test.js, isolation.spec.ts                |
| R6     | User can delete item                               | Backend + E2E             | shopItem.test.js, shop.spec.ts                     |

Note: These are core functional requirements only.