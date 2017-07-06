Links: https://github.com/home-assistant/home-assistant

# Overview
This document will describe a proposed addition to Home Assistant to add multi user authentication and authorization.  Home Assistant (Hass) up to this point has focused on the single user use case.  It expects all users of the system to authentication with the same credentials.  It also has an API authentication use case, which means all machines using the API use the same credentials as well.  Adding support for multiple users, and giving those user access to different resoures within the system. Some user stories to get started:

1. An owner of a Hass setup would like to remove all the extra feature and panels from their spose, giving them access to just controlling the components.

2. An owner would like to prevent their roomate from being able to turn their bedroom lights on and off, but still give them access to controlling the common room lights.

3. An owner would like to delegate authentication to a third party (OpenId / OAuth provider).

With these goal in mind, I'd like to propose the following changes to the Hass web interface.
