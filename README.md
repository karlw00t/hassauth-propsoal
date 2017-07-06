Links: https://github.com/home-assistant/home-assistant

# Overview
This document will describe a proposed addition to Home Assistant to add multi user authentication and authorization.  Home Assistant (Hass) up to this point has focused on the single user use case.  It expects all users of the system to authentication with the same credentials.  It also has an API authentication use case, which means all machines using the API use the same credentials as well.  Adding support for multiple users, and giving those user access to different resoures within the system. Some user stories to get started:

1. An owner of a Hass setup would like to remove all the extra feature and panels from their spose, giving them access to just controlling the components.

2. An owner would like to prevent their roomate from being able to turn their bedroom lights on and off, but still give them access to controlling the common room lights.

3. An owner would like to delegate authentication to a third party (OpenId / OAuth provider).

# Glossery

Authentication - The act of a user present crednetial and verifying they are who they say they are.

Authorization - The act of asserting that an Authenticated user can do a specific action.

Admin - A entity in Hass that has no limit on there permissions (kind to root in Linux)

User - A entity in Hass that can get restricted access components within the system.

# Interface changes

The default home assistant experience will go unchanged.  The default, unauthenticated web page will give users an easy on boarding.  If auth is enabled on your home assistant instance, you will be required to login to the server.  Bootstrapping the user database requires Hass be launched in a trusted environment.  That is, the first person to connect to the server in "auth" mode gets to set up the first user.  From there, the first user can add new users as needed.  This would be accomplished through the "Admin Users" panel.  Users can also be set to "Admin" in this panel, to allow for more than one Admin

There will be an option in the "Admin Users" panel to delegate authentication to a reverse proxy.  This would allow advanced users to set up a proxy in front of there Hass server to authentication users.  If this option is selected, only user names are needed to be populated in the "Admin Users" panel, passwords would not be required.

There will be a "User" panel that will allow indidiviual users to manage their own passwords.  There could be other information in this panel, mobile number, email address, etc. for notifications.

There will be a "Admin Permissions" Panel that will allow Adminstrator to set permissions on users.  Assuming there are two users in Hass, John and Parker.  There are three components in the system, bedroom light 1, bedroom light 2, and garage door.  The permission page would look like this

|        | Bedroon light 1 | Bedroom light 2 | Garage Door |
|--------|:---------------:|:---------------:|:-----------:|
| Parker | ✔               |       ✔         |             |
| John   |                 |                 |      ✔      |

Admins would be able to check an uncheck the components that the users have access to.  In the above example, Parker and see and modify the Bedroom light 1 and 2, and John can see and modify the Garage Door.

# Internals
One of the underlying assumptions for this is system is that authentication and authorization are done in seperate systems.  After a user has been authenticated, a header is injected into the HTTP request with the username of the authenticate user.  The Authorization system picks up on this header and does the correct checks to ensure the user on is allow to do what they are permitted to do.

##Authentication
I'm sure there is some code out there that we can ~~steal~~ borrow for this.  This will be a simple authentication mechanism, which will use cookies to store tokens.  To start, authentication tokens last forever.

##Authorization
I'm going to have to do some more research as to how Hass brings from the frontend to the backend server, but this component will take the injected username from the headers, and make descisions on what action are allowed, what data can be seen.  
