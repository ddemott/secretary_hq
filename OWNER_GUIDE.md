# SecretaryHQ — Owner's Guide

A plain-language guide for business owners: what each dashboard tab does, how to
read your call analytics, and answers to the questions owners ask most. No
technical background needed.

---

## The dashboard at a glance

When you log in you land on **Home**. The top navigation has two groups:

**Everyday tabs** (everyone on your team sees these):

- **Home** — today's snapshot: upcoming appointments, recent calls, quick stats.
- **Schedule** — the calendar. Book, move, or cancel appointments; see each
  staff member's day.
- **Contacts** — your address book (this tab used to be called Customers).
  Every caller the AI talks to is saved here, not just the ones who booked.
  Notes and preferences live on each contact. Owners can edit or remove a
  contact's saved preferences; front-desk staff can see them but not change
  them. The list can be searched (name, email, phone digits, tags, and old
  names), sorted, and filtered. When you rename a contact, the old name is kept
  and shown as "Also heard as" on the card. Each contact's card shows its
  **call history** with a summary and whether a transcript exists. Owners can
  delete a contact: it is hidden and future appointments are cancelled, but
  the history is kept.
- **Calls** — the log of every call your AI receptionist handled: who called,
  what they wanted, whether it booked, and the full transcript.

**Owner tabs** (only owners see these):

- **Setup** — everything about how your business is configured, as sub-tabs:
  Services, your resources (bays, chairs, rooms, vans — named the way your
  trade names them), your staff, Working Days (the weekly schedule grid), Who
  Can Do What, Team Access (staff logins and roles: Owner or Front Desk),
  Business Settings (business profile, greeting details), Billing, Audit Log,
  and the Answer Debugger. The **Setup Assistant** button re-opens the guided
  setup wizard, which also has a short mode for one-person businesses.
- **Phone Assistant** — how your AI sounds and behaves, including the
  **AI Persona** page (voice, speaking style, greeting, "Forward Calls to a
  Person," and the caller-preferences toggle). It also holds your call
  checklist: you can turn parts off, mark fields optional or required, and
  preview what the next call will ask.

> Front-desk logins only see the everyday tabs. If a front-desk user opens an
> owner-only link, they're sent back to Home.

**Want a walkthrough?** Open the account menu (the person icon, top right) and
choose **Take the product tour**. It steps through every tab and switches tabs
for you; press Esc to leave at any time. Front-desk logins get a shorter tour.
New owners are offered the tour after setup. The account menu also lets you
pick a color **theme** (for example Navy, Midnight, or High Contrast).

**Just looking?** The website's **Tutorial** gives you a private sample
business, with sample calls and messages, to click through. It resets in place
and expires on its own.

---

## Your business starts from a ready-made setup

When your account is created you pick your type of business (auto shop, salon,
plumber, bakery, and so on) and your account starts as a copy of a ready-made
example business of that type. The copy is yours; nothing is shared with other
businesses.

- **Services** typical for your type of business, with sensible lengths, but
  **no prices.** We never set your prices; add your own, or leave them off.
- **Your bays, chairs, rooms, or vans**, named the way your trade names them.
- **Two placeholder staff**, named for your trade (for example "Mechanic 1"),
  already matched to the services they can do. Rename them to your real people
  or remove them.
- **Knowledge-base starters** — a few common questions and answers. They are
  marked **"Not used yet — edit and save"**: the AI will not read one to a
  caller until you have checked it and saved it.

You never get customers, calls, appointments, or opening hours; those are
always your own. If you change your business type before finishing the setup
wizard, the example items are swapped for the new type's; after that,
everything is yours.

---

## How to read your call analytics

The analytics panels (on the Calls / Home area) turn raw calls into four
answers. All of them are driven by real call records — nothing is estimated.

### Call Volume

How many calls your AI handled over the period. Use it to spot busy days/weeks
and to confirm the assistant is actually receiving traffic. A sudden drop to
zero usually means a phone-routing problem, not a quiet week — check with your
operator.

### Booking Conversion

Of the calls that came in, how many ended in a booked appointment. A call counts
as "booked" when it created an appointment. This is your headline number: it
tells you how much revenue the assistant is capturing.

- **Low conversion** doesn't always mean the AI failed — many calls are
  questions, not booking attempts. Read it alongside "Why Callers Reached Out".

### Caller Abandonment

Calls that ended without a booking **and** without a clear reason captured. A
high abandonment number is the one to watch — it can mean callers are hanging up
because they couldn't get what they needed. Pair it with the transcripts in the
**Calls** tab to hear what happened.

### Why Callers Reached Out

A breakdown of what non-booking callers actually wanted, sorted into:

- **No availability** — they wanted a time you didn't have open. _Action:_
  consider extending staff shifts (your open hours are the union of staff
  shifts) or adding capacity.
- **Wrong service** — they asked for something you don't offer. _Action:_ maybe
  a service to add, or clearer messaging.
- **Price** — they were shopping on price.
- **Message** — they left a message for a person.
- **Info** — a general question (hours, location, policies).

This panel is the most useful for decisions: it tells you _why_ calls didn't
book, so you know whether to change hours, add services, or improve your
knowledge base.

---

## Making the AI answer questions correctly

Your AI answers caller questions ("Do you take walk-ins?", "What's your
cancellation policy?") from a **knowledge base** you control:

1. During setup you can paste your website URL and the assistant proposes
   answers for you to review. It never publishes them on its own, and it can
   re-check your site from time to time for new suggestions.
2. You can add or edit individual Q&A entries anytime, and upload documents
   (PDF, text, Word, Markdown) for it to draw policies from.
3. If the AI ever gives a wrong or "I don't know" answer, an owner can use the
   built-in **Answer Debugger** (Setup → Answer Debugger) to see exactly which knowledge entries the AI
   considered for a question and how strongly each matched — so you know whether
   to add or reword content.

If a caller asks something the knowledge base doesn't cover, the AI says it
doesn't have that information rather than guessing, and the question is logged so
you can add an answer later.

---

## Frequently asked questions

**Does the AI book real appointments on my calendar?**
Yes. Confirmed bookings are written to your Schedule immediately, and the system
prevents double-booking the same staff member or resource.

**Will it take a caller's preferred time?**
Yes — the assistant asks the caller's preferred day/time, offers open slots, and
widens to the next window if nothing fits. It never imposes a slot.

**What happens if a caller wants a human?**
If you've set a forward number (Phone Assistant → AI Persona → "Forward Calls to
a Person"), the AI can transfer the live call to that number.

**Do callers get reminders?**
Confirmation and reminder emails send, subject to the customer's consent. Text
reminders depend on carrier registration for your business's number and are not
active yet, so the assistant does not promise a text. Customers can also cancel
or reschedule from a self-service link.

**Is every caller saved, even if they don't book?**
Yes. The assistant identifies callers and saves them to your Contacts address
book, so a caller who just asked a question is still a lead you can follow up.

**Who can see what?**
Owners see everything. Front-desk logins see the everyday tabs (Home, Schedule,
Contacts, Calls) but not business configuration, team management, or the AI
settings. Sensitive views like the change-history audit log are owner-only.

**Can I export my data?**
Yes — owners can download a complete export of their business data (contacts,
appointments, call history, knowledge base, and more).

**How do I change the AI's voice or greeting?**
Phone Assistant → AI Persona. You can set the voice, speaking speed/style, and
the greeting per business.

**Does the AI remember what a caller likes?**
Yes, by default. When a caller mentions something lasting (a preferred stylist,
"weekends only"), the AI saves it for that caller and brings it up next time,
only for a caller it can actually confirm. Turn it off on Phone Assistant → AI
Persona.

**What does the free trial involve?**
New accounts confirm their email first, and the 14-day free trial needs a card
on file. Plans include a set number of calls a month, and your line keeps
answering past that (extra calls are billed). See Setup → Billing for usage and
current plan details.

---

_Questions this guide doesn't answer? The Calls transcripts are the best place to
hear exactly how a specific conversation went._
