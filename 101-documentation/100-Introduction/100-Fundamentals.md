### Architectural Fundamentals

In database jargon, PostgreSQL use a client/server modell. A PostgreSQL session consists of the following cooperating processes (programs):

- A server process, which manages the databases files, accepts connections to the database from client applications, and preforms database actions on behalf of the clients. The database server program is called **postgres**.
- The user's client (frontend) application that wants to perfrom database operations. Client applications can be very diverse in nature: a client could be a text-oriented tool, a graphical application, a web server that accesses the database to display web pages, or a specialized database maintenance tool. Some client applications are supplied with the PostgreSQL distribution; most ar developed by users.
