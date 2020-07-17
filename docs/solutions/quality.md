# Quality Standards

These are seen as goals, more like a vision, meaning not every goal may be reached or completely reached. But you should try to fulfill these as much as possible.

## Functionality

!!! check "Goal"

    Everything which is possible in the web should be allowed. The range should be kept wide but implemented in a short range first.

Each function have to fulfill the following points:

-   all specific requirements have to be reached
-   security issues have to be checked
-   automatic tests should be integrated

## Universal usable

!!! check "Goal"

    The system should run on most systems without problems. Special modules are always optional.

No commercial code is used or integrated. General interfaces are used everywhere:

-   easy to install
-   works beside other software

For the user it should also be accessible with any client on any system from everywhere with ease:

-   easy to understand
-   easy to learn
-   use as known from other systems
-   optimized for office (shortcuts...)

## Stability

!!! check "Goal"

    Clear control structures and optimal error management helps to work on problems. Unit Tests give the ability to find those early in the development process.

As bugs are always there this is reached specifically by:

-   complete checking of all external data
-   one problem should not bring down the whole system
-   work with problems and broken connections and retry automatically

## Performance

!!! check "Goal"

    The system's performance is optimized and it should be as fast as possible also under high frequency.

But this is not the highest goal because it contradicts the fast prototyping.

-   low resource usage
-   support for different caching systems
-   clustering with load balancing
-   monitoring to detect problems before they occur

## Extensibility

!!! check "Goal"

    Modularization is used to make extending the system an easy task.

Loose coupling and dependency injection are the key concepts to allow easy extension. The concrete way differs based on the language possibilities.

## Security

!!! check "Goal"

    The user's data is the valueablest thing, so always protect it.

Nowadays security is one of the biggest point everywhere. So you should:

-   connections and data transfer should always be secure
-   passwords and sensitive data is only stored secured
-   user identification are done through anonymous tokens
-   statistics should never contain user specific data
-   user data can also be deleted from the system on demand

{!docs/abbreviations.txt!}
