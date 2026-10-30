# Corban

A black-and-white, photo-led website for the Corban movement. The current site uses plain HTML, CSS, and JavaScript, with three pages and no build step, framework, backend, database, or installed dependencies.

## How the website works

- The browser loads each page as a separate HTML document.
- `styles.css` provides shared typography, layout, colors, image cropping, and mobile rules.
- `app.js` injects the same navbar and footer into every page, highlights the active navigation link, handles the cart dialog, and handles the contact form.
- Images load directly from `dist/assets/`. The site uses system fonts.
- The footer year updates automatically in the browser.
- JavaScript is required for the navbar, footer, cart, and contact-form behavior. The page text and images are in the HTML.

## Where to edit

| File | Responsibility |
| --- | --- |
| `dist/index.html` | Homepage hero, short statements, community section, and quote |
| `dist/about.html` | Corban meaning, story, Christian roots, and nine values |
| `dist/contact.html` | Contact-page copy, fields, and status messages |
| `dist/styles.css` | Shared styles and responsive layout; mobile rules start at `@media(max-width:700px)` |
| `dist/app.js` | Shared navbar/footer templates, destination settings, cart dialog, and contact handler |
| `dist/assets/logo.png` | Cropped logo used in navbar and footer |
| `dist/assets/corban.png` | Original supplied logo, retained as a reference |
| `dist/assets/city.jpg` | Homepage hero image |
| `dist/assets/friends.jpg` | Homepage community image |

The HTML, CSS, and JavaScript are currently compacted into long lines. Format the file in your editor before making substantial edits, then review the diff to distinguish formatting from behavior changes.

## Run locally

From the repository root, using Python 3:

```sh
python3 -m http.server 8000 --directory dist
```

Open <http://localhost:8000>. Stop the server with Ctrl+C. No `npm install` or `npm run build` is needed for this version.

## Set up the shop and contact channel

Edit the configuration at the top of `dist/app.js`:

```js
const CORBAN_CONFIG = {
  shopUrl: '',      // Client-approved full HTTPS shop/cart destination
  contactEmail: ''  // Client-approved email address
};
```

- With an empty `shopUrl`, Cart opens a coming-soon dialog. With a URL, it redirects the visitor to that destination in the same tab.
- This site does not maintain a shopping cart, inventory, checkout, or payments. Those belong to the linked shop.
- With an empty `contactEmail`, submitting a valid form displays a message explaining that nothing was sent.
- With an email address configured, submission opens the visitor's email app with the topic, name, message, and reply address filled in. The visitor must still send the email; the website cannot confirm delivery.
- After configuring email, update the coming-soon copy and preview status text in `dist/contact.html` so the page accurately describes the behavior.
- For direct delivery without an email app, integrate a form service or backend and replace the submit handler. That functionality is not implemented.
- All browser JavaScript is visible to visitors. Never put passwords, private API keys, or service credentials in this configuration.

## Code review checklist

- Keep `<div data-header></div>` and `<div data-footer></div>` on every page: `app.js` expects both elements and replaces them with the shared templates.
- Keep the deferred script tag and stylesheet link on every page. A missing or broken script also removes the shared navigation and footer.
- When renaming a page or moving an asset, update every matching `href` and `src`, including links inside the JavaScript templates. Paths and filename capitalization must match exactly.
- When changing contact fields, keep their `name` attributes aligned with the `FormData` lookups in `app.js`: `name`, `email`, `topic`, and `message`.
- Preserve the form's required fields, email input type, labels, and `.status` element with `role="status"`.
- Test Cart with the destination empty and configured. Test closing its dialog with the button, Escape, and a click outside its content.
- Test the form with invalid input, valid input while unconfigured, and configured email. Opening an email app is not proof an email was sent.
- Check the browser console for JavaScript errors and the Network tab for missing files or failed requests.
- Check all three pages at phone, tablet, and desktop widths. Look for horizontal overflow, crowded navigation, clipped text, and awkward image crops.
- Use the keyboard to reach links, Cart, dialog controls, and form fields. Check visible focus, understandable labels, readable contrast, and image alt text.
- Review changes to shared styles on all pages; one selector can affect several sections.
- Keep dynamic visitor input out of `innerHTML`. The current templates are fixed strings; form values are encoded into the email URL.
- Review `git diff` before committing. Keep temporary archives, logs, environment files, and credentials out of Git.

## Before public launch

- Obtain the final shop URL and contact address, configure them, and test both flows.
- Replace or explicitly approve the AI-generated campaign images. They do not depict actual Corban members or verified merchandise.
- Confirm the client's approval of the logo, page copy, quotations, and use of photography.
- The supplied “Needs” passage was incomplete; its current summary uses the supplied theme of generosity. Ask the client to approve or provide the remainder.
- Confirm the contact-page messaging matches the configured behavior.
- Review titles, descriptions, social-sharing metadata, and favicon requirements before promotion. Social-preview metadata and a favicon have not been added to this version.
- Perform a real browser visual review. JavaScript syntax and local link/asset checks passed during preparation; browser visual QA was not completed.

## Hosting and deployment

- Publish the contents of `dist/` as the website root on a static host. `dist/` is the editable source for this buildless version, despite its conventional name.
- No build command is required. If the host requests an output/publish directory, use `dist`.
- The published ChatGPT Sites preview is <https://corban-movement.goodsnipe2.chatgpt.site>. Access remains governed by that hosting service.
- A GitHub commit does not by itself update the ChatGPT Sites preview. No deployment automation has been added to this repository.
- If an external host was previously configured for Astro, review its build and publish settings before deploying this version.

## Previous version and repository cleanup

- The previous Astro implementation is preserved on the `legacy/astro-site` branch, including its components, configuration, and assets.
- `main` contains the current static site and this guide. The old Astro dependencies, storefront demo pages, and unrelated product assets are no longer part of the active tree.
- Git history is preserved. This update does not rewrite earlier commits.
