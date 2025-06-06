# DEPLOY THE FREE TEMPLATE (RONALDO BOOTSTRAP 4 PORTFOLIO TEMPLATE)
Editing the template to match my portfolio information, add manual JavaScript and CSS scripts and remove sections that is not needed. The java scripts were used to show real time to the users as they are in my portfolio website.

- Download the Bootstrap 4 portfolio template from https://themewagon.com/themes/free-bootstrap-4-html5-one-page-personal-portfolio-website-template-ronaldo/
- Exact the zip file using 7zip
- Open the index.html

## EDIT THE CUSTOM TEMPLET TO MATCH MY CONTENT
We can edit this section by typing <span class= “subheading”>
First edit to add my name and small animation with rotation
![images](images/image19.png)

Next section
### We can edit this section by typing <p class= “about=text”>
This section is an about me page where brief about me and strengths
![images](images/image20.png)

### We can edit this section by typing <uI Class=” about-info mt-4 px-md-0 px-2”>
This section is basic personal contact details
![images](images/image21.png)

### Editing the top page menu
We can edit this section by typing <nav id="navi">
![images](images/image22.png)

## UPDATING EDUCATION SECTION
We can edit this section by typing <div class="text pl-3">
![images](images/image23.png)

![images](images/image24.png)

![images](images/image25.png)

![images](images/image26.png)

## UPDATING EXPERIENCES SECTION
### We can edit this section by typing <div id="page-2" class= "page two">
![images](images/image27.png)

![images](images/image28.png)

![images](images/image29.png)

![images](images/image30.png)

![images](images/image31.png)

## UPDATING THE SKILLS ECTION
### We can edit this section by typing <div id="page-3" class= "page three">
![images](images/image32.png)

![images](images/image33.png)

![images](images/image34.png)

## NOW UPDATE HORIZONTAK SKILLS
### We can edit this section by typing  <div class="col-md-6 animate-box">
![images](images/image35.png)

![images](images/image36.png)

![images](images/image37.png)

![images](images/image38.png)

![images](images/image39.png)

## REMOVE THE AWARDS SECTION WITH CERTIFICATE SECTION
### We can edit this section by typing <div id="page-4" class="page four">
Manual html scripting

![images](images/image40.png)


## UPDATE EACH PROJECT ACCORDINGLY (MANUAL HTML SCRIPTING)
### Removed all the default projects and replaced it with mine

![images](images/image41.png)
- Added all the certification according to certificates.
- Next, I removed the service section, blog section since this is my first portfolio

### We can edit this section by typing <section class="ftco-section ftco-hireme img"
![images](images/image42.png)

## Next section similar but more details, such as address, phone number and website.
### We can edit this section by typing <div class="row d-flex contact-info mb-5">
![images](images/image43.png)

## UPDATING HEADER AT THE END
### We can edit this section by typing <ul class="list-unstyled">
![images](images/image44.png)
- Replace services at end of the page with relevant skills

### We can edit this section by typing <div class="block-23 mb-3">
![images](images/image45.png)

### Replaced the licensing of the page and used my licensing but credited colorlib for using their template
![images](images/image46.png)

## FIXING INCONSISTENT FONTS IN EDUCATION SECTION
### Manual CSS script 
Open ccs folder in the root folder then open style.css and scroll to the very bottom.
h2 {
font-weight: 700 !important;

## LIVE TIME DISPLAY CLOCK (JAVA SCRIPT)
### Description 
A custom live digital clock was displayed on the site. This clock uses java scripting, and the clock updates every second using the function setInterval() and formats time using its built-in date object.

#### In index.html add
- <div id="clock">00:00:00</div> 
- Add it just below <body data-spy="scroll" data-target=".site-navbar-target" data-offset="300"> you can can edit this section by typing that scrip as well.

#### Then in the <script> section at the bottom of the page (above </body>), add:
For easy access you can type </body> using ctrl+f function

<script>
   function ShowsTheCurrentTime() {
      const TimeCurrent = new Date ();
      const TimeFormatted = TimeCurrent.toLocaleTimeString();
    document.getElementById('clock').textContent = TimeFormatted;
  }
  setInterval(ShowsTheCurrentTime, 1000);
  ShowsTheCurrentTime(); // Run at start
</script>

#### We also need to updater the style.css to do that open ccs folder in the root folder then open style.css and scroll to the very bottom. Paste this code to position and style the clock
}
#clock {
  position: fixed;
  top: 10px;
  left: 10px;
  background-color: transparent;
  color: black;
  font-size: 16px;
  font-family: 'Poppins', sans-serif;
  z-index: 9999;
}
