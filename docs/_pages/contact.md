---
layout: page
title:  Contact
description: Connect with me on Linkedin or drop me a message
date:   2026-11-22 15:01:35 +0300
form_title: ...Or drop me a message here!
---

<!-- begin linkedin connect split section -->
<section class="section contact-linkedin-hero">
  <div class="container">
    <div class="row">
      <div class="col col-12">
        <div class="contact-linkedin-split">

          <div class="contact-linkedin-split__left">
            <h2 class="section__title">Let's connect on LinkedIn!</h2>
            <p class="contact-linkedin-split__text">
              Join my network on LinkedIn to stay up to date with my recent projects, industry insights, and career milestones. 
              Feel free to send a connection request or start a conversation there to share some good ideas.
            </p>
            <a class="c-button c-button--large c-contact-form__linkedin-btn"
               href="https://www.linkedin.com/in/gabriele-casarin06/"
               target="_blank"
               rel="noopener noreferrer">
              <i class="ion ion-logo-linkedin" aria-hidden="true"></i>
              <span>LinkedIn</span>
            </a>
          </div>

          <div class="contact-linkedin-split__right">
            <div class="contact-linkedin-split__image">
              <img loading="lazy" src="https://raw.githubusercontent.com/gcasarin/gcasarin.github.io/main/images/contact_image.gif" alt="Connect on LinkedIn">
            </div>
          </div>

        </div>
      </div>
    </div>
  </div>
</section>
<!-- end linkedin connect split section -->

<!-- begin contact -->
<div class="c-contact-form" id="contact">
  <div class="container">
    <div class="row">
      <div class="col col-12">

        <div class="c-contact-form__form-box">
          <div class="c-contact-form__contact-head">
            <h2 class="c-contact-form__contact-title">{{ page.form_title | default: site.data.settings.contact_form__settings.form_title }}</h2>
            <p class="c-contact-form__contact-description">{{ site.data.settings.contact_form__settings.form_description }}</p>
          </div>
          <form class="c-contact-form__form"
            action="{{ site.data.settings.contact_form__settings.form_action_url }}"
            method="POST">
            <div class="c-contact-form__form-group">
              <label class="c-contact-form__form-label screen-reader-text" for="form-name">Your Name</label>
              <input class="c-contact-form__form-input" id="form-name" type="text" name="name" placeholder="Your name..." required>
            </div>
            <div class="c-contact-form__form-group">
              <label class="c-contact-form__form-label screen-reader-text" for="form-email">Your Email</label>
              <input class="c-contact-form__form-input" id="form-email" type="email" name="_replyto" placeholder="Your email..."
                required>
            </div>
            <div class="c-contact-form__form-group">
              <label class="c-contact-form__form-label screen-reader-text" for="form-text">Your Message</label>
              <textarea class="c-contact-form__form-input" id="form-text" name="text" rows="9" placeholder="Your message..."
                required></textarea>
            </div>
            <div class="c-contact-form__form-group c-contact-form__form-group--button">
              <button class="c-button c-button--primary c-button--large" type="submit">{{ site.data.settings.contact_form__settings.form_button_text }}</button>
            </div>
          </form>
        </div>

      </div>
    </div>
  </div>
</div>
<!-- end contact -->
