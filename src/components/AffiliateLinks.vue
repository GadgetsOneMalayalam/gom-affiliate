<template>
<div>
    <div class="contenedorCards">
        <div class="card" v-for="link in affiliateLinks" :key="link.id">
            <div :href="link.url" class="wrapper">
                <div class="colorProd"></div>
                <a class="imgProd"  target="_blank" :href="link.url" :style="`background-image: url(${link.image});`"></a>
                <div class="infoProd">
                    <p class="nombreProd"><a target="_blank" :href="link.url">{{ link.title }}</a></p>
                    <p class="extraInfo" v-if="link.extraInfo">{{link.extraInfo}}</p>
                    <div class="actions" :class="{'no-price' : !link.mrp && !link.offerPrice}">
                    <div class="preciosGrupo" v-if="link.mrp || link.offerPrice">
                        <p class="precio precioOferta" v-if="link.mrp">{{ link.mrp }}</p>
                        <p class="precio precioProd" v-if="link.offerPrice">{{ link.offerPrice }}</p>
                    </div>
                    <!-- <a target="_blank" :href="link.url" class="bakuretsu_icono action alCarrito">
                        <svg class="inCart" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64">
                          <title>Go to Product</title>
                          <path d="M30 22H12M2 6h6l10 40h32l3.2-9.7"></path>
                          <circle cx="20" cy="54" r="4"></circle>
                          <circle cx="46" cy="54" r="4"></circle>
                          <circle cx="46" cy="22" r="16"></circle>
                          <path d="M53 18l-8 9-5-5"></path>
                        </svg>
                    </a> -->
                    <a class="action youtube-video" v-if="link.youtubeVideo" :href="link.youtubeVideo" target="_blank">
                        <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" viewBox="0 0 48 48">
                            <path class="outer" fill="url(#PgB_UHa29h0TpFV_moJI9a_9a46bTk3awwI_gr1)" d="M45.012,34.56c-0.439,2.24-2.304,3.947-4.608,4.267C36.783,39.36,30.748,40,23.945,40        c-6.693,0-12.728-0.64-16.459-1.173c-2.304-0.32-4.17-2.027-4.608-4.267C2.439,32.107,2,28.48,2,24s0.439-8.107,0.878-10.56        c0.439-2.24,2.304-3.947,4.608-4.267C11.107,8.64,17.142,8,23.945,8s12.728,0.64,16.459,1.173c2.304,0.32,4.17,2.027,4.608,4.267        C45.451,15.893,46,19.52,46,24C45.89,28.48,45.451,32.107,45.012,34.56z"></path><path class="inner1" d="M32.352,22.44l-11.436-7.624c-0.577-0.385-1.314-0.421-1.925-0.093C18.38,15.05,18,15.683,18,16.376        v15.248c0,0.693,0.38,1.327,0.991,1.654c0.278,0.149,0.581,0.222,0.884,0.222c0.364,0,0.726-0.106,1.04-0.315l11.436-7.624        c0.523-0.349,0.835-0.932,0.835-1.56C33.187,23.372,32.874,22.789,32.352,22.44z" opacity=".05"></path><path class="inner2" d="M20.681,15.237l10.79,7.194c0.689,0.495,1.153,0.938,1.153,1.513c0,0.575-0.224,0.976-0.715,1.334        c-0.371,0.27-11.045,7.364-11.045,7.364c-0.901,0.604-2.364,0.476-2.364-1.499V16.744C18.5,14.739,20.084,14.839,20.681,15.237z" opacity=".07"></path><path class="inner3" fill="#fff" d="M19,31.568V16.433c0-0.743,0.828-1.187,1.447-0.774l11.352,7.568c0.553,0.368,0.553,1.18,0,1.549        l-11.352,7.568C19.828,32.755,19,32.312,19,31.568z"></path>
                        </svg>
                    </a>
                    <div class="icono action aFavs" @click="link.isLiked=!link.isLiked" :class="{'esFav': link.isLiked}">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64">
                        <path d="M47 5c-6.5 0-12.9 4.2-15 10-2.1-5.8-8.5-10-15-10A15 15 0 0 0 2 20c0 13 11 26 30 39 19-13 30-26 30-39A15 15 0 0 0 47 5z">
                        </path>
                        </svg>
                    </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div class="footer-text">The products listed on this page contain affiliate links. When you purchase any product, GadgetsOneMalayalam may earn a commision. <b>It won't cost you extra.</b></div>
</div>
</template>
  
<script>
import axios from "axios";

export default {
name: 'AffiliateLinks',
data() {
    return {
    affiliateLinks: [],
    };
},
mounted() {
    axios
    .get(
        "https://docs.google.com/spreadsheets/d/1BoUXqR3xZxE1AlTcPdSSlrvFpOVfBP3Pg1nMMiCeZ4Q/gviz/tq?tqx=out:csv"
    )
    .then((response) => {
        const data = response.data.split("\n");
        this.affiliateLinks = data.slice(1).map((row, index) => {
        const rowData = row.split(",");      // Remove quotes around values (if any)
        const cleanedRowData = rowData.map((value) => {
            if (value.startsWith('"') && value.endsWith('"')) {
            return value.substring(1, value.length - 1);
            } else {
            return value;
            }
        });
        return {
            id: index + 1,
            title: cleanedRowData[0],
            image: cleanedRowData[1],
            url: cleanedRowData[2],
            mrp: cleanedRowData[3],
            offerPrice: cleanedRowData[4],
            extraInfo: '',
            isLiked: false,
            youtubeVideo: cleanedRowData[5],
        };
        });
    })
    .catch((error) => {
        console.error("Error fetching data:", error);
    });
},
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Roboto+Condensed:wght@300&family=Roboto:wght@300;400;900&display=swap");
:root {
  --currencyPrefix: "₹";
}

body {
  overflow-x: hidden;
  color: #666;
  left: 0;
  padding: 0;
  margin: 0 auto;
  position: relative;
  transition: ease all 0.3s;
}
body * {
  font-family: "Roboto Condensed", sans-serif;
}
body p, body h1, body h2, body h3, body h4, body h5, body h6 {
  margin: 0;
}
body a {
  color: #666;
  text-decoration: none;
}
body ul, body li {
  padding: 0;
  margin: 0;
  list-style-type: none;
}

.contenedorCards {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  align-content: center;
  min-height: 100vh;
  padding: 20px 0;
  box-sizing: border-box;
}
.contenedorCards .card {
  width: 300px;
  transition: ease all 0.3s;
}
.contenedorCards .card.enCarrito .wrapper .infoProd .actions .action.alCarrito .inCart {
  transform: scale(1);
}
.contenedorCards .card.enCarrito .wrapper .infoProd .actions .action.alCarrito .outCart {
  transform: scale(0);
}
.contenedorCards .card .wrapper {
  display: block;
  margin: 60px 10px 10px 10px;
  padding-top: 300px;
  box-sizing: border-box;
  position: relative;
  box-shadow: 0 0 20px 10px rgba(29, 29, 29, 0.1);
  transition: ease all 0.3s;
}
.contenedorCards .card .wrapper:hover {
  transform: translateY(-10px);
}
.contenedorCards .card .wrapper:hover .imgProd {
  height: 350px;
  display:  block;
}
.contenedorCards .card .wrapper .colorProd {
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 200px;
}
.contenedorCards .card .wrapper .imgProd {
  background-size: contain;
  background-position: center bottom;
  background-repeat: no-repeat;
  position: absolute;
  bottom: calc(100% - 300px);
  width: 100%;
  height: 300px;
  transition: ease all 0.3s;
  display:  block;
}
.contenedorCards .card .wrapper .infoProd {
  display: flex;
  flex-direction: column;
  flex-wrap: nowrap;
  align-items: center;
  justify-content: center;
  align-content: center;
  height: 130px;
  padding: 20px;
  box-sizing: border-box;
}
.contenedorCards .card .wrapper .infoProd p {
  width: 100%;
  font-size: 14px;
  text-align: center;
}
.contenedorCards .card .wrapper .infoProd .nombreProd {
  margin-bottom: 10px;
}
.contenedorCards .card .wrapper .infoProd .nombreProd a {
  font-family: "Roboto", sans-serif;
  font-size: 16px;
  font-weight: 600;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
}
.contenedorCards .card .wrapper .infoProd .extraInfo {
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}
.contenedorCards .card .wrapper .infoProd .actions {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  align-content: center;
  width: 100%;
  margin-top: auto;
  padding-top: 10px;
}
.contenedorCards .card .wrapper .infoProd .actions.no-price {
  justify-content: center;
}

.contenedorCards .card .wrapper .infoProd .actions .preciosGrupo {
  flex-grow: 1;
  position: relative;
}
.contenedorCards .card .wrapper .infoProd .actions .preciosGrupo .precio {
  font-family: "Roboto", sans-serif;
  color: #1d1d1d;
  font-size: 25px;
  font-weight: 600;
  text-align: left;
}
.contenedorCards .card .wrapper .infoProd .actions .preciosGrupo .precio.precioOferta {
  position: absolute;
  left: 0;
  top: -15px;
  color: red;
  font-size: 15px;
  text-decoration: line-through;
}
.contenedorCards .card .wrapper .infoProd .actions .preciosGrupo .precio.precioOferta:before {
  font-size: 12px;
}
.contenedorCards .card .wrapper .infoProd .actions .preciosGrupo .precio:before {
  content: var(--currencyPrefix);
  font-size: 20px;
}
.contenedorCards .card .wrapper .infoProd .actions .action {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  align-content: center;
  margin-left: 15px;
  width: 35px;
  height: 35px;
  position: relative;
  transition: cubic-bezier(0.68, -0.55, 0.27, 1.55) all 0.3s;
  cursor: pointer;
  color: #1d1d1d;
}
.contenedorCards .card .wrapper .infoProd .actions .action svg {
  position: absolute;
  transition: cubic-bezier(0.68, -0.55, 0.27, 1.55) all 0.3s;
}
.contenedorCards .card .wrapper .infoProd .actions .action svg path, .contenedorCards .card .wrapper .infoProd .actions .action svg circle {
  stroke: currentColor;
  fill: transparent;
  transition: ease all 0.3s;
}
.contenedorCards .card .wrapper .infoProd .actions .action.youtube-video {
    width: 30px;
    height: 30px;
    color: #b2071d;
}

.contenedorCards .card .wrapper .infoProd .actions .action.youtube-video:hover path.outer {
    fill: #b2071d;
}

.contenedorCards .card .wrapper .infoProd .actions .action.youtube-video:hover path.inner3 {
    fill: #fff;
}

.contenedorCards .card .wrapper .infoProd .actions .action.aFavs {
  width: 25px;
  height: 25px;
  color: red;
}
.contenedorCards .card .wrapper .infoProd .actions .action.aFavs.esFav {
  transform: rotateX(360deg) scale(1.2);
}
.contenedorCards .card .wrapper .infoProd .actions .action.aFavs.esFav svg path,
.contenedorCards .card .wrapper .infoProd .actions .action.aFavs.esFav svg circle {
  fill: red;
  transition-delay: 0.2s;
}

.footer-text {
  margin-top: 3rem;
  margin-bottom: 3rem;
}

</style>
