
# Introduction

Instead of using the request-response paradigm, the WebSocket protocol allows for full-duplex (i.e., bi-directional) message transmissions between servers and clients without any prior request from the other party. Such WebSocket connections typically remain open for an extended period and allow for data transmission anytime in any direction.

For example, let's consider a simple HTTP/1.1 chat room web application running in the browser of two participants, `Alice` and `Bob`. When `Alice` sends a message to `Bob`, her browser transmits the message to the web server; however, the web server will not be able to send the message to `Bob` simultaneously because it cannot send a message without a prior request. Thus, Bob's browser must periodically `poll` the web server for new messages from Alice, and depending on the number of messages transmitted to Bob, this mechanism creates much traffic and could be inefficient.

![[Pasted image 20250328160016.png]]
Suppose the same application uses WebSocket connections instead. In that case, Alice and Bob will establish a WebSocket connection with the web server upon login. Afterward, Alice's browser will simultaneously transmit her messages to Bob via the WebSocket connection without polling requests. Thus, WebSockets are highly advantageous for real-time applications.

Suppose the same application uses WebSocket connections instead. In that case, Alice and Bob will establish a WebSocket connection with the web server upon login. Afterward, Alice's browser will simultaneously transmit her messages to Bob via the WebSocket connection without polling requests. Thus, WebSockets are highly advantageous for real-time applications.