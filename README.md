# Project Conversation History

## 1. Ensure to have necessary dependencies

You must have docker and docker compose installed on your machine

Then you can execute the following commands

```
docker compose up --build
```

## 2. Execute the Application

Go to localhost:3000

Press Login and then enter the credentials

```
email: test@gmail.com
password: password
```

## 3. Execute the Specs

In order to run tests execute the following commands
```
rspec ./spec
```


## 4. Conversation

**[09:00 AM] Sean:**  
Morning! Let’s finalize the real-time experience. Starting with typing indicators?

**[09:01 AM] Abubakr:**  
Yep. Emit `user_typing` via Turbo Streams for now. Debounced + throttled?

**[09:01 AM] Sean:**  
Yes. Trigger on keypress, cancel after 5s inactivity.

**[09:02 AM] Abubakr:**  
Okay, we won’t persist typing events — just in-memory broadcasts.

**[09:03 AM] Sean:**  
Perfect. Next: message CRUD. We’ve got Create/Read. What about edit?

**[09:04 AM] Abubakr:**  
Let’s allow edits within 5 minutes. Store `edited_at` timestamp, show `(edited)` in the UI.

**[09:05 AM] Sean:**  
Makes sense. I’ll update the message component to diff old vs new content on edit.

**[09:06 AM] Abubakr:**  
For Delete: soft delete only. Replace text with “This message was deleted”.

**[09:07 AM] Sean:**  
Got it. We’ll still keep ID + timestamp for sorting and delivery markers.

**[09:08 AM] Abubakr:**  
You’ll need to watch for `message_updated` and `message_deleted` Turbo Stream events too.

**[09:09 AM] Sean:**  
Yep. Working now. Message components auto-refresh on any mutation.

**[09:10 AM] Abubakr:**  
What about delivery indicators? Should we support “sent”, “delivered”, and “seen”?

**[09:11 AM] Sean:**  
Let’s track:  
- **Sent** = created in DB  
- **Delivered** = sent via Turbo/AnyCable to recipient  
- **Seen** = user scrolls to message + window has focus

**[09:12 AM] Abubakr:**  
Nice. Add Redis TTL cache to track last seen per thread. Keep DB clean.

**[09:13 AM] Sean:**  
Smart. I’ll show delivery ticks like WhatsApp: ✓ = sent, ✓✓ = delivered, ✓✓ (blue) = seen

**[09:14 AM] Abubakr:**  
How about ordering? Use server timestamps only?

**[09:15 AM] Sean:**  
Yes. I’ll display messages sorted by `created_at`, not client receive time.

**[09:16 AM] Abubakr:**  
Great. Add microsecond precision — avoids issues on fast threads.

**[09:17 AM] Sean:**  
Done. Are we ready to scale this?

**[09:18 AM] Abubakr:**  
Currently Turbo handles 1K clients okay, but I’m containerizing AnyCable next.

**[09:19 AM] Sean:**  
AnyCable-Go or Pro?

**[09:19 AM] Abubakr:**  
Go for now — open source, low latency, gRPC backend support.

**[09:20 AM] Sean:**  
Should we separate chat room logic by channel? `ChatRoomChannel` per thread?

**[09:21 AM] Abubakr:**  
Yep. Namespaced streams like `chat_rooms:<id>:messages` make it easy to isolate.

**[09:22 AM] Sean:**  
Want to support replies and threads?

**[09:22 AM] Abubakr:**  
Later. For now, flat structure. But keep `parent_message_id` in schema for nesting.

**[09:23 AM] Sean:**  
Okay. I’ll reserve thread UI space but hide it for MVP.

**[09:24 AM] Abubakr:**  
Also — rate limit message send per user to 3/sec. Avoid abuse.

**[09:25 AM] Sean:**  
I’ll add client-side throttling and disable the input briefly after sends.

**[09:26 AM] Abubakr:**  
Backend’s using Redis token bucket — so we’re safe there too.

**[09:27 AM] Sean:**  
Last thing — do we want message search in MVP?

**[09:28 AM] Abubakr:**  
Not MVP, but I’ll index messages with trigram search in Postgres for later.

**[09:29 AM] Sean:**  
Cool. Let’s test this whole flow today. I’ll simulate 50 users on my end.

**[09:30 AM] Abubakr:**  
Awesome. I’ll watch Redis, logs, and DB under load. Let’s ship soon 💬


