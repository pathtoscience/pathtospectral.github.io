---
layout: default
title: Contact
---

<div style="text-align: center; margin-bottom: 2rem;">
    <h1>Get in Touch</h1>
    <p>I am always open to discussing new research collaborations and opportunities.</p>
</div>

<form id="contact-form" action="https://formspree.io/f/xykdynkj" method="POST">
    <div class="form-group">
        <label for="name">Name</label>
        <input type="text" id="name" name="name" placeholder="Your Name" required>
    </div>

    <div class="form-group">
        <label for="email">Email</label>
        <input type="email" id="email" name="email" placeholder="your@email.com" required>
    </div>

    <div class="form-group">
        <label for="message">Message</label>
        <textarea id="message" name="message" rows="5" placeholder="How can we collaborate?" required></textarea>
    </div>

    <button type="submit" id="submit-btn" class="btn">Send Message</button>
    <div id="status-message" class="form-status"></div>
</form>

<script>
    const form = document.getElementById("contact-form");
    const status = document.getElementById("status-message");
    const submitBtn = document.getElementById("submit-btn");

    form.addEventListener("submit", async function(event) {
        event.preventDefault();
        submitBtn.disabled = true;
        submitBtn.innerText = "Sending...";
        
        const data = new FormData(event.target);
        
        try {
            const response = await fetch(event.target.action, {
                method: form.method,
                body: data,
                headers: {
                    'Accept': 'application/json'
                }
            });
            
            if (response.ok) {
                status.innerHTML = "Success! Your message has been sent. I'll get back to you soon.";
                status.className = "form-status success";
                status.style.display = "block";
                form.reset();
            } else {
                const result = await response.json();
                status.innerHTML = result.errors ? result.errors.map(error => error.message).join(", ") : "Oops! There was a problem submitting your form.";
                status.className = "form-status error";
                status.style.display = "block";
            }
        } catch (error) {
            status.innerHTML = "Oops! There was a problem submitting your form.";
            status.className = "form-status error";
            status.style.display = "block";
        } finally {
            submitBtn.disabled = false;
            submitBtn.innerText = "Send Message";
        }
    });
</script>
