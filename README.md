## Database MySQL

### User

* `id`, `username`, `email`, `password`, `voice_notifications_enabled`,
  `created_at`
* All fields are self explanatory.
* `voice_notifications_enabled` is set to true/false depending on the user's
  input while registration.
* `password` is not saved in plain string, instead its md5 is saved.
* While login, user can input its username or email and password. Method:
  `ApplicationController.php`->`validateCredentials`
* User registration -> `ApplicationController.php`->`addUser`

### Bid Item

* `id`, `name`, `description`, `created_by`, `status`, `created_at`,
  `duration_mins`, `base_amount`
* All fields are self explanatory.
* `status`= `running`|`closed`.
* `duration_mins` minutes for which the item is open for auction.
* If there is no bid on the item for those number of minutes, item stays in
  `running` status until bids are placed.
* `base_amount` minimum amount of bid for the item.
* Add new item -> `ApplicationController.php`->`addProduct`

### Bid

* `id`, `bid_item_id`, `created_by`, `created_at`, `updated_at`, `amount`
* All fields are self explanatory.
* `amount` Bid placed in $ by user on any item.
* User can edit his/her bid until the auction is running. `updated_at` changes
  everytime the bid amount is updated.
* Bid placed by the user should be greater than `base_amount` of item.
* Bid placed by the user should be current highest bid on that item at that
  time.
* `base_amount` minimum amount of bid for the item.
* Add new bid -> `ApplicationController.php`->`addBid`

### Notification

* `id`, `user_id`, `title`, `description`, `entity_id`, `entity_type`,
  `is_read`, `created_at`
* All fields are self explanatory.
* `entity_id` shows for what entity, the notification is created for. Like
  `bid_item`
* `entity_type` entity's type.
* `is_read` is a boolean value, set to true when the notification is read by the
  user. We show the unread count badge using this value.
* If `voice_notifications_enabled` is `true` for user, System fetches the unread
  (`is_read`: 0) notification and started reading them one by one and also set
  `is_read` true for them, so that they are not retrived next time.

### Logout disabled for User

* If any item created by the logged in user is in running state, user is not
  allowed to logout.
* If user is currently winner for any running bid, he/she is not allowed to
  logout.
* Login is in `layout.php`.

## Server: Nginx

* Basics of nginx and how to configure:
  https://www.linode.com/docs/web-servers/nginx/how-to-configure-nginx/

## Backend language: PHP.

## Frontend: HTML, CSS, Javascript, jQuery

## Cloud deployment

* AWS EC2:
  * https://aws.amazon.com/ec2/?nc2=h_l3_c
  * http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html

## Video conferencing: WebRTC

* https://webrtc.org/
* https://codelabs.developers.google.com/codelabs/webrtc-web/#0
* We are using WebRTC protocol to run the conferencing part of our application.
* It setups multiple peer connections to support multi-user connectivity
  feature.
* Realtime signalling is being handled by a third party server and is done by
  Socket.js.
* Basically, we are creating a room with username. All other users can see
  already created rooms and can join the room to see each other's video/audio.
