<template>
    <v-app id="inspire">
        <v-app-bar app elevation="0">
            <v-subheader><img height="100%" src="../src/assets/add-logo.png"/></v-subheader>
            <v-tabs center-active right background-color="transparent" active-class="active-tab">
                <v-tab href="#home">Home</v-tab>
                <v-tab>Community</v-tab>
                <v-tab>Charts</v-tab>
                <v-tab>About</v-tab>
                <v-tab>Contact</v-tab>
            </v-tabs>
            <v-btn large color="blue" class="white--text ml-10">Upgrade Now</v-btn>
        </v-app-bar>
        <v-main style="background-color: #F5F5F5">
            <section class="intro-section" id="home">
                <v-row class="align-center px-10" style="height: 690px">
                    <v-col lg="6" cols="6" class="pa-6 font-weight-bold py-16">
                        <h2 class="display-3">Insightful, <span class="green--text">Visual</span> & <span class="red--text">Quick</span> <span class="green--text">to</span>
                            Access</h2>
                        <br><br>
                        <v-subheader>The Africa Digital Databank (ADD) is an integrated repository of multiple world databases, allowing users to seamlessly browse any country for comparative visualisations and global analyses over three distinct themes – Economics, Development and Governance.</v-subheader>
                        <br>
                        <v-dialog
                            v-model="dialog"
                            width="100%"
                            height="100%"
                            >
                            <template v-slot:activator="{ on, attrs }">
                                <v-btn
                                large elevation="5" class="white--text mt-10 px-10 py-5"
                                color="green"
                                dark
                                v-bind="attrs"
                                v-on="on"
                                >
                                Start Building
                                </v-btn>
                            </template>

                            <v-card>
                                <v-card-title class="text-h5 grey lighten-2">
                                Find some data
                                </v-card-title>
                                <v-container class="grey lighten-5">
                                    <!-- <Dashboard/> -->
                                    <v-row no-gutters>
                                    <v-col>
                                        <v-card
                                        class="pa-2"
                                        tile
                                        >
                                        Country/Region
                                        </v-card>
                                    </v-col>
                                    <v-col>
                                        <v-card
                                        class="pa-2"
                                        tile
                                        >
                                        Data Source
                                        </v-card>
                                    </v-col><br>
                                    <v-col>
                                        <v-card
                                        class="pa-2"
                                        tile
                                        >
                                        Indicator
                                        </v-card>
                                    </v-col>
                                    </v-row>
                                </v-container>

                                <v-divider></v-divider>

                                <v-card-actions>
                                <v-spacer></v-spacer>
                                <v-btn
                                    large elevation="5" class="white--text"
                                    color="green"
                                    @click="dialog = false"
                                >
                                    Generate Chart
                                </v-btn>
                                </v-card-actions>
                            </v-card>
                        </v-dialog>
                        <!-- <v-btn large elevation="5" color="green" class="white--text mt-10 px-10 py-5">
                            Start Building
                        </v-btn> -->
                        <v-btn large elevation="5" color="red" class="white--text mt-10 ml-10 px-10 py-5">30-day free trial</v-btn>
                        <v-row class="mt-16">
                            <v-chip class="px-4 ma-3" color="dark">
                                <v-icon left size="20">mdi-facebook</v-icon>
                                Facebook
                            </v-chip>
                            <v-chip class="px-4 ma-3" color="dark">
                                <v-icon left size="20">mdi-twitter</v-icon>
                                Twitter
                            </v-chip>
                            <v-chip class="px-4 ma-3" color="dark">
                                <v-icon left size="20">mdi-linkedin</v-icon>
                                Linkedin
                            </v-chip>
                            <v-chip class="px-4 ma-3" color="dark">
                                <v-icon left size="20">mdi-whatsapp</v-icon>
                                WhatsApp
                            </v-chip>
                        </v-row>
                    </v-col>
                    <v-col lg="6" cols="6">
                        <v-img src="../src/assets/home-data.png"></v-img>
                    </v-col>
                </v-row>
            </section>
            <section class="about-me" id="about">
                <v-img class="pa-10" height="400" gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
                       src="https://www.seasiainfotech.com/blog/wp-content/uploads/2018/02/History-Evolution-of-Cloud-Computing.png">
                    <v-row justify="center" class="align-center">
                        <v-col lg="6" cols="6">
                            <v-card class="text-center pa-6 rounded-lg" color="#e1e0e0c9" elevation="6">
                                <v-card-subtitle class="green--text"><h2 class="font-weight-strong">ABOUT THE DATABANK</h2></v-card-subtitle><br>
                                <p class="font-weight-normal">
                                    The Africa Digital Databank allows you to explore multiply databases under three distinct themes. 
                                    The themes allow you to browse the data seamlessly across multiple databases. 
                                    The databank allows you to use the time-series data for analysis and visualization on countries around the globe. 
                                </p>
                                <v-btn large elevation="5" color="green" class="white--text mt-10 px-10 py-5">Download
                                    DATA 
                                    <v-icon>mdi-download</v-icon>
                                </v-btn>
                            </v-card>
                        </v-col>
                        <v-col lg="6" cols="6">
                            <v-card class="pa-6 rounded-lg" color="#e1e0e0c9" elevation="6">
                                <v-card-subtitle class="black--text"><h2 class="font-weight-strong">THE TECHNOLOGY WE USE</h2></v-card-subtitle>
                                <div class="py-1" v-for="(item,index) in items" :key="index">
                                    <label>{{ item.title }}</label>
                                    <v-progress-linear
                                            :color="item.color"
                                            height="10"
                                            :value="item.value"
                                    ></v-progress-linear>
                                </div>
                            </v-card>
                        </v-col>
                    </v-row>
                </v-img>
            </section>
            <section class="educaion py-6">
                <v-container>
                    <h4 class="display-2 text-center">Themes we exlpore</h4>
                    <v-row class="py-8">
                        <v-col cols="12" lg="4">
                            <v-card class="pa-6 rounded-xl " elevation="5">
                                <div class="d-flex align-center justify-space-between">
                                    <div class="green--text">
                                        <h2>Development</h2>
                                    </div>
                                    <a class="red--text"><v-icon color="red">mdi-link</v-icon>Explore</a>
                                </div>
                                <v-card-text>
                                    The development theme contains the most current development and social data 
                                    such as education, health, and gender equality. 
                                </v-card-text><br><br>
                            </v-card>
                        </v-col>
                        <v-col cols="12" lg="4">
                            <v-card class="pa-6 rounded-xl " elevation="5">
                                <div class="d-flex align-center justify-space-between">
                                    <div class="green--text">
                                        <h2>Economy</h2>
                                    </div>
                                    <a class="red--text"><v-icon color="red">mdi-link</v-icon>Explore</a>
                                </div>
                                <v-card-text>
                                    The economy theme contains timeseries data relating to economic and financial measures 
                                    such as gross domestic product, government spending, economic growth and much more. 
                                </v-card-text><br>
                            </v-card>
                        </v-col>
                        <v-col cols="12" lg="4">
                            <v-card class="pa-6 rounded-xl " elevation="5">
                                <div class="d-flex align-center justify-space-between">
                                    <div class="green--text">
                                        <h2>Governance</h2>
                                    </div>
                                    <a class="red--text"><v-icon color="red">mdi-link</v-icon>Explore</a>
                                </div>
                                <v-card-text>
                                    The governance theme contains timeseries data measuring the governance performance of countries 
                                    across various indicators including rule of law, effectiveness, 
                                    control of corruption, and much more. 
                                </v-card-text>
                            </v-card>
                        </v-col>
                    </v-row>
                </v-container>
            </section>
            <section class="experiance py-6">
                <v-img class="pa-10" gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
                       src="https://images.techhive.com/images/article/2016/08/cloud-computing-data-center-100678075-large.jpg">
                    <v-container>
                        <h4 class="white--text display-2 text-center">Things we do with the data</h4>
                        <v-timeline>
                            <v-timeline-item v-for="(exper,index) in experience" :key="index"
                                             :color="index % 2 === 0 ?'pink':'green'">
                                <v-card class="rounded-xl">
                                    <v-card-title :color="index % 2 === 0 ?'pink':'green'">
                                        {{exper.title}}
                                    </v-card-title>
                                    <v-card-text>
                                        <b>{{exper.date}}</b><br>
                                        <b>by the {{exper.institute}} team</b><br><br>
                                        {{exper.decription}}
                                    </v-card-text>
                                </v-card>
                            </v-timeline-item>
                        </v-timeline>
                    </v-container>
                </v-img>

            </section>
            <section class="databank">
                <h4 class="display-2 text-center">Data Sources</h4>
                <v-slide-group
                        class="pa-4"
                        active-class="success"
                        show-arrows
                >
                    <v-slide-item v-for="(slider,index) in sliders" :key="index">
                        <v-card height="250" width="250" class="mx-2">
                            <img height="100%" width="100%" :src="slider" alt="">
                        </v-card>
                    </v-slide-item>
                </v-slide-group>
            </section>
            <section class="contact-us pt-10">
                <v-img class="pt-10" gradient="to bottom, rgb(166 199 156), rgb(227 227 227 / 86%)" src="https://img.freepik.com/free-photo/contact-us_36325-2135.jpg?size=626&ext=jpg">
                    <v-container>
                        <a href="https://gga.org">
                            <img style="padding: 0 40% 0 40%;" height="200px" src="https://digitalmallblobstorage.blob.core.windows.net/wp-content/2021/07/GGA-logo-Full-Colour-Pantone.png"/>
                        </a>
                        <p class="text-center mb-5" style="font-size:23px;">
                            Good Governance Africa is a research and advocacy non-profit organisation 
                            with centres across Africa focused solely on improving governance across 
                            the continent.
                        </p>
                        <p class="text-center mb-5" style="font-size:23px;">
                            GGA engages in applied research and stimulates critical debate. 
                            All our work is based on exploring and advancing the key governance principles of democracy, 
                            accountability and transparency, and combining these with upholding the rule of law and 
                            respecting human, civil and property rights.
                        </p>
                        <v-divider />
                        <br>
                        <v-row>
                            <v-col cols="12" lg="6">
                                <v-card-title>SEND US A MESSAGE</v-card-title>
                                <v-form>
                                    <v-text-field label="Your Name" />
                                    <v-text-field label="Email" />
                                    <v-text-field label="Subject" />
                                    <v-textarea label="Message" />
                                    <v-btn large color="green" class="white--text">Send Message</v-btn>
                                </v-form>
                            </v-col>
                            <v-col cols="12" lg="3" offset="2">
                                <v-card-title>CONTACT US</v-card-title><br>
                                <v-card-text>
                                    <v-icon>mdi-domain</v-icon> The Mall Offices, <br>
                                    11 Cradock Avenue, <br>
                                    Rosebank, 2196 <br>
                                    Johannesburg <br>
                                    South Africa <br><br>
                                    <v-icon>mdi-phone</v-icon> +27 11 268 0479 <br><br>
                                    <v-icon>mdi-fax</v-icon> +27 11 268 0478 <br><br>
                                    <v-icon>mdi-email</v-icon> data@gga.org <br>
                                </v-card-text>
                                <!-- <v-card-actions>
                                    <v-btn large elevation="5" color="pink" class="white--text mt-10 px-10 py-5">Download
                                        Resume
                                        <v-icon>mdi-download</v-icon>
                                    </v-btn>
                                </v-card-actions> -->
                            </v-col>
                        </v-row>
                    </v-container>
                    <v-footer class="mt-4">
                        <v-col
                                class="text-center"
                                cols="12"
                        >
                            <strong>Copyright ©{{ new Date().getFullYear() }} All rights reserved | Developed by
                                <a href="" style="text-decoration: none;" class="green--text">Good Governance Africa NPC</a>
                            </strong>
                        </v-col>
                    </v-footer>
                </v-img>
            </section>
        </v-main>
    </v-app>
</template>

<script>
// import Dashboard from "@/components/dashboard"

    export default {
        name: 'App',

        components: {
        //   Dashboard,
        },

        data: () => ({
            drawer: null,
            items: [
                {title: 'AZURE', value: 100, color: 'light-blue'},
                {title: 'SQL DB', value: 100, color: 'deep-orange'},
                {title: 'Cognitive Search', value: 90, color: 'black'},
                {title: 'Javascript', value: 70, color: 'light-green'},
                {title: 'Vuejs', value: 80, color: 'blue'},
            ],
            experience: [
                {
                    title: 'Conflict in Africa monitor',
                    date: 'March 2020 - Present',
                    institute: 'Human Security and Climate Change',
                    decription: 'GGA’s Conflict in Africa Monitor (CAM) is a project designed to track and analyse major conflict hotspots across the continent, with the objective of providing decision makers with the data and tools necessary to develop informed policy solutions to mitigate or end political violence and conflict. Navigate within the map of Africa to access conflict location and event data, regular situation reports, and deeper level analysis by country of interest.'
                },
                {
                    title: 'Sentiment Barometer',
                    date: 'March 2020 - Present',
                    institute: 'Governance Delivery and Impact',
                    decription: 'The purpose of the Governance Barometer is to host a geographically visualised repository of news articles on issues relating to the scope of GGA’s research programmes. This serves to deepen researchers’ understanding of governance issues across Africa. Most importantly, it provides an opportunity for researchers to engage in a more critical assessment of the standard or mainstream reporting narrative.'
                },
                {
                    title: 'Governance Coefficient',
                    date: 'March 2020 - Present',
                    institute: 'Governance Insights and Analytics',
                    decription: 'The Governance Coefficient assesses the quality of governance of African countries in a similar way to the Gini Coefficient. The latter is calculated with reference to the Lorenz Curve as a measure of the level of inequality within a society on a scale from 0 to 1 where 1 represents the highest level of income inequality and 0 represents the most equal distribution of income.'
                },
                {
                    title: 'Intelligence Reports',
                    date: 'March 2020 - Present',
                    institute: 'Natural Resource Governance',
                    decription: 'What problem do we solve with IRs? Intelligence Reports are published monthly to propel the private sector towards becoming development partners rather than mere ‘policy-takers’ or ‘rent extractors’. We aid private sector decision-making towards enhancing Environmental, Social and Governance (ESG) performance. It is our considered view that businesses which mainstream ESG principles authentically into their core business strategies will do better than their competitors in the long run. This strengthens our commitment at Good Governance Africa (GGA) to promote good governance and the rule of law across the continent.'
                },
            ],
            sliders:[
                'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQbCyBm5st1RAjnQoA9CwhyrsQj0lAl97i5mSKZdogVyNROdmk-DyotkkqFeE3Hpnr86ms&usqp=CAU',
                'https://yt3.ggpht.com/Ng577aobad6q36P04MiJCuX2Y0ZIXikl2gKIjVkI0z5ef-dkr0wlL056RNQhDT-ye-8sTmmx=s900-c-k-c0x00ffffff-no-rj',
                'data:image/png;base64,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',
                'http://www.systemicpeace.org/africa/csplogo.gif',
                'https://libapps-au.s3-ap-southeast-2.amazonaws.com/accounts/75637/images/UNStats.png',
            ]
        }),
    }
</script>
<style>
    .v-tab.active-tab {
        color: #e91e63;
    }

    .v-tab.active-tab::before {
        background-color: #4caf50;
    }
</style>