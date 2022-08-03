<script>
  import '@/assets/base.css';
  export default {

    data() {
      return {

        title: 'SoftChair',
        describe: 'calming thing for each member of your family',
        img: 'https://img.buzzfeed.com/buzzfeed-static/static/2017-12/20/16/asset/buzzfeed-prod-fastlane-01/sub-buzz-29109-1513806488-1.jpg',
        productsFirst: [
          {
            price: 100,
            button: 'Add to bucket',
            active: false,
          },
        ],

        titleSecond: 'GranddadyChair',
        describeSecond: 'feel the warmth and comfort, start to relax',
        imgSecond: 'https://m.media-amazon.com/images/I/81VAyE4WYTL._AC_SL1500_.jpg',
        productsSecond: [
          {
            price: 200,
            button: 'Add to bucket',
            active: false,
          },
        ],
        
        elapsed: 0,
        duration: 9.99*100000,

      }
    },

    created() {
      let lastTime = performance.now();
      const update = () => {
        const time = performance.now();
        this.elapsed += Math.min(time - lastTime, this.duration - this.elapsed);
        lastTime = time;
        this.handle= requestAnimationFrame(update);
      }
      update()
    },

    unmouted() {
      cancelAnimationFrame(this.handle);
    },

    methods: {

      toggleActive(e) {
        return e.active = !e.active;
      },
      total: function() {
        let total = 0;
        this.productsFirst.forEach(function(e) {
          if(e.active) {
            total += e.price;
            if(total > 50) {
              let cardFirst = document.getElementById("cardFirst");
              cardFirst.style.display = "block";
            }
            if(total < 100) {
              let cardFirstSale = document.getElementById("cardFirstSale");
              cardFirstSale.style.display = "block";
            }
          } 
        })
        this.productsSecond.forEach(function(e) {
          if(e.active) {
            total += e.price;
            let cardSecond = document.getElementById("cardSecond");
            cardSecond.style.display = "block";
          }
        }) 
        return total;
      },
      close: function() {
        let cardFirst = document.getElementById("cardFirst");
        cardFirst.style.display = "none";
        this.productsFirst.forEach(function(e) {
          return e.active = false;
        })

        let cardFirstSale = document.getElementById("cardFirstSale");
        cardFirstSale.style.display = "none";
        this.productsFirst.forEach(function(e) {
          return e.active = false;
        })

        let cardSecond = document.getElementById("cardSecond");
        cardSecond.style.display = "none";
        this.productsSecond.forEach(function(e) {
          return e.active = false;
        })
      },
      sale() {
        this.productsFirst.forEach(function(e) {
          return e.price = e.price/2;
        })
      }      
    },

    watch: {

    },

  }

</script>

<template>

  <!-- ---------- MENU AND PRODUCT CARDS -->

        <main>

            <div class="container">
                <header class="blog-header lh-1 py-3" style="border-bottom: 1px solid;">
                  <div class="row flex-nowrap justify-content-between align-items-center">
                    <div class="col-4 pt-1">
                      <a class="link-secondary" href="#">Subscribe</a>
                    </div>

                    <div class="col-4 text-center"><a href="http://localhost:3000/" class="blog-header-logo text-dark name">Furniture Store</a></div>

                    <div class="col-4 d-flex justify-content-end align-items-center">
                      <button class="bucketButton btn btn-danger collapsed btn btn-sm btn-outline-secondary" data-bs-toggle="modal" data-bs-target="#exampleModal"><img src="https://cdn-icons-png.flaticon.com/512/190/190544.png" class="bucketImage"><p class="totalPrice">Total price: {{total()}}$</p></button>
                    </div>
                  </div>
                </header>

                <div class="nav-scroller py-1 mb-2">
                  <nav class="nav d-flex justify-content-between">
                    <a class="p-2 link-secondary" href="#">World</a>
                    <a class="p-2 link-secondary" href="#">Technology</a>
                    <a class="p-2 link-secondary" href="#">Culture</a>
                    <a class="p-2 link-secondary" href="#">Business</a>
                    <a class="p-2 link-secondary" href="#">Politics</a>
                    <a class="p-2 link-secondary" href="#">Science</a>
                    <a class="p-2 link-secondary" href="#">Health</a>
                    <a class="p-2 link-secondary" href="#">Style</a>
                    <a class="p-2 link-secondary" href="#">Travel</a>
                  </nav>
                </div>
            </div>

          <div class="album py-5 bg-light">

            <div class="container">

              <div class="timer" style="margin-top: -40px; width: 347px">
                <progress :value="elapsed/duration"></progress >
                <p style="text-align: center;"><i>until the end of the sale: {{999 - Math.round(((elapsed/1000).toFixed(0)))}} second</i></p>
                <input type="range" v-model="duration" min="0" max="999000" style="display:none"/>
              </div>

              <div class="row row-cols-1 row-cols-sm-2 row-cols-md-2 g-3">

                <div class="col">
                  <div id="product" class="card shadow-sm ">
                    <button id="buttonSale" class="buttonSale" @click.once="sale()">Sale</button>
                    <img class="bd-placeholder-img card-img-top img h-70" width="100%" :src="img" />
                    <div class="card-body">
                      <p class="card-text title">{{title}}</p>
                      <p class="card-text describe">{{describe}}</p>
                      <div class="showPrice" v-for="e in productsFirst">
                        <button class="button btn btn-danger" @click="toggleActive(e)">{{e.button}}</button>
                      </div>
                    </div>
                  </div>
                </div>

                <div class="col">
                  <div id="product" class="card shadow-sm">
                    <img class="bd-placeholder-img card-img-top img" width="100%"  :src="imgSecond" />
                    <div class="card-body">
                      <p class="card-text title">{{titleSecond}}</p>
                      <p class="card-text describe">{{describeSecond}}</p>
                      <div class="showPrice" v-for="e in productsSecond">
                        <button class="button btn btn-danger" @click="toggleActive(e)">{{e.button}}</button>
                      </div>
                    </div>
                  </div>
                </div>

              </div>
            </div>
          </div>

        </main>

  <!-- ---------- END PRODUCT CARDS -->

  <!-- ---------- MODAL WINDOW -->

        <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
          <div class="modal-dialog modal-dialog-centered">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel">Bucket:</h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
              </div>
              <div class="modal-body">
                <p>You ordered: </p>
                <ul id="cardFirst" style="display: none;">
                  <li>One "SoftChair" priced at $100</li>
                </ul>
                <ul id="cardFirstSale" style="display: none;">
                  <li>One "SoftChair" priced at $50</li>
                </ul>
                <ul id="cardSecond" style="display: none;">
                  <li>One "GranddadyChair" priced at $200</li>
                </ul>
              </div>
              <p style="margin-left: 16px; margin-bottom: -10px;"><strong>Total price: {{total()}}$</strong></p><br/>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @click="close()">Reset</button>
              </div>
            </div>
          </div>
        </div>

  <!-- ---------- END MODAL WINDOW -->

  <!-- ---------- FOOTER -->

        <div class="container">
          <footer class="py-5">
            <div class="row">
              <div class="col-6 col-md-2 mb-3">
                <h5>Section</h5>
                <ul class="nav flex-column">
                  <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">Home</a></li>
                  <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">Features</a></li>
                  <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">Pricing</a></li>
                  <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">FAQs</a></li>
                  <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">About</a></li>
                </ul>
              </div>

            <div class="col-6 col-md-2 mb-3">
              <h5>Section</h5>
              <ul class="nav flex-column">
                <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">Home</a></li>
                <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">Features</a></li>
                <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">Pricing</a></li>
                <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">FAQs</a></li>
                <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">About</a></li>
              </ul>
            </div>

            <div class="col-6 col-md-2 mb-3">
              <h5>Section</h5>
              <ul class="nav flex-column">
                <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">Home</a></li>
                <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">Features</a></li>
                <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">Pricing</a></li>
                <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">FAQs</a></li>
                <li class="nav-item mb-2"><a href="#" class="nav-link p-0 text-muted">About</a></li>
              </ul>
            </div>

            <div class="col-md-5 offset-md-1 mb-3">
              <form>
                <h5>Subscribe to our newsletter</h5>
                <p>Monthly digest of what's new and exciting from us.</p>
                <div class="d-flex flex-column flex-sm-row w-100 gap-2">
                  <label for="newsletter1" class="visually-hidden">Email address</label>
                  <input id="newsletter1" type="text" class="form-control" placeholder="Email address">
                  <button class="btn btn-primary" type="button">Subscribe</button>
                </div>
              </form>
            </div>
          </div>

          <div class="d-flex flex-column flex-sm-row justify-content-between py-4 my-4 border-top">
            <p>&copy; 2022 Company, Inc. All rights reserved.</p>
            <ul class="list-unstyled d-flex">
              <li class="ms-3"><a class="link-dark" href="#"><svg class="bi" width="24" height="24"><use xlink:href="#twitter"/></svg></a></li>
              <li class="ms-3"><a class="link-dark" href="#"><svg class="bi" width="24" height="24"><use xlink:href="#instagram"/></svg></a></li>
              <li class="ms-3"><a class="link-dark" href="#"><svg class="bi" width="24" height="24"><use xlink:href="#facebook"/></svg></a></li>
            </ul>
          </div>
        </footer>
      </div>

  <!-- ---------- END FOOTER -->

</template>

