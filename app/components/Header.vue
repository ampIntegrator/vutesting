<template>
        <header id="header">
            <div class="headerTop">
                <div class="wrapper">
                    <div class="flexCSB">
                        <NuxtLink to="/index" class="logo">
                            <img src="/images/logo-green.svg" alt="">
                        </NuxtLink>
                        <ul class="menu">
                            <li><NuxtLink to="/index">Cotisations & avantages</NuxtLink></li>
                            <li><NuxtLink to="/notre-histoire">Cité verte</NuxtLink></li>
                            <li><NuxtLink to="/qui-sommes-nous">Le Végétal, c'est la vie</NuxtLink></li>
                        </ul>
                        <div class="headerActionDesktop">
                            <NuxtLink to="/index" class="btn btnOutline espacePro"><span>Espace Pro</span><i class="icon icon-arrow-right"></i></NuxtLink>
                            <NuxtLink to="/index" class="whoTrigger btn btnOutline"><span>Vous êtes ?</span><i class="icon icon-priority-low"></i></NuxtLink>
                            <NuxtLink to="/index" class="searchTrigger btn btnOutline"><i class="icon icon-magnifier"></i></NuxtLink>
                        </div>
                        <div class="headerActionMobile">
                            <NuxtLink to="/index" class="btnFull espacePro"><i class="icon icon-suitcase"></i></NuxtLink>
                            <NuxtLink to="/index" id="" class="whoTrigger btnFull"><i class="icon icon-user"></i></NuxtLink>
                            <NuxtLink to="/index" id="" class="searchTrigger btnFull"><i class="icon icon-magnifier"></i></NuxtLink>
                            <NuxtLink to="/index" id="" class="searchTrigger btnFull"><i class="icon icon-menu"></i></NuxtLink>
                        </div>
                    </div>
                </div>
            </div>
            <div class="headerBottom">
                <div class="wrapper">
                        <ul class="ulWithSub">
                            <li><span>Le Végétal &<br>les métiers du futur</span></li>
                            <li><span>L'interprofession<br>VALHOR</span></li>
                            <li><span>Valoriser<br>la filière</span></li>
                            <li><span>Filière<br>durable</span></li>
                            <li><span>Le marché<br>du Végétal</span></li>
                            <li><span>Faire pousser<br>l'excellence</span></li>
                        </ul>
                </div>
            </div>
            <div class="stickyHeader">
                <div class="wrapper">
                    <div class="flexCSB">
                        <div class="flexCS">
                            <NuxtLink to="/index" class="logo">
                                <img src="/images/logo-green-scroll.svg" alt="">
                            </NuxtLink>
                        <ul class="ulWithSub">
                            <li><span>Le Végétal &<br>les métiers du futur</span></li>
                            <li><span>L'interprofession<br>VALHOR</span></li>
                            <li><span>Valoriser<br>la filière</span></li>
                            <li><span>Filière<br>durable</span></li>
                            <li><span>Le marché<br>du Végétal</span></li>
                            <li><span>Faire pousser<br>l'excellence</span></li>
                        </ul>
                        </div>
                        <div class="headerActionDesktop">
                            <NuxtLink to="/index" class="btn btnOutline espacePro"><span>Espace Pro</span><i class="icon icon-arrow-right"></i></NuxtLink>
                            <NuxtLink to="/index" class="whoTrigger btn btnOutline"><span>Vous êtes ?</span><i class="icon icon-priority-low"></i></NuxtLink>
                            <NuxtLink to="/index" class="searchTrigger btn btnOutline"><i class="icon icon-magnifier"></i></NuxtLink>
                        </div>
                    </div>
                </div>
            </div>
            <div class="headerSubMenu">
                <div class="wrapper">
                    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Ut tenetur nobis neque dolores cum sunt dolor consequuntur ex velit laborum labore magnam, nesciunt repudiandae voluptatem? Reiciendis, vel explicabo. Dolore ducimus obcaecati tempore culpa adipisci unde at! Excepturi, mollitia! Fugit ipsa minima aut nulla minus odit neque? Quo totam necessitatibus repellat eaque, exercitationem itaque maiores nemo consequatur expedita quod dolor aliquid fuga quos amet ratione tempore officiis ipsa! Doloremque quos id incidunt cumque alias magnam! Alias fugit consequuntur similique eos blanditiis deleniti tempora, animi qui doloribus adipisci id, cum maxime quae ut impedit nihil libero nostrum quis quibusdam vitae modi officiis?</p>
                </div>
            </div>
        </header>
</template>

<style lang="scss" scoped>
.espacePro {
    i.icon.icon-arrow-right {
        transform: rotate(-45deg);
    }
}

// HEADER TOP
#header {
    box-shadow: 0 1rem 3rem rgba(0, 0, 0, 0.175);
    .headerTop {
        background-color: white;
        display: flex;
        align-items: center;
        transition: $trans;

        * {
            color: $main
        }

        height: 76px;
    }

    .headerBottom {
        background-color: $main;
        transition: $trans;
        .wrapper {
            ul {
                max-width: 1000px;
                margin: 0 auto;
                display: flex;
                align-items: center;
                transition: $trans;

                li {
                    flex-grow: 1;
                    height: 76px;
                    display: flex;
                    align-items: center;
                    justify-content: start;
                    transition: $trans;
                    text-align: center;

                    span {
                        display: block;
                        width: 100%;
                        color: white;
                    }

                    &.active {
                        background-color: $light;

                        span {
                            color: $main
                        }
                    }
                }
            }
        }
    }

    &.header-is-green {
        .headerTop {
            background-color: $main;

            * {
                color: white;

            }

            .menu {
                li {
                    a {
                        &:hover {
                            border-bottom-color: white;
                        }
                    }
                }
            }

            .btn {
                border-color: white;

                &:hover {
                    background-color: white !important;

                    span,
                    i {
                        color: $main !important
                    }
                }
            }
        }

        .headerBottom {
            background-color: white;

            ul {
                li {
                    span {
                        color: $ink;
                    }
                }
            }
        }
    }

}

.logo {
    img {
        height: 44px;
    }
}

ul {
    &.menu {
        display: flex;
        align-items: center;
        justify-content: start;
        transition: $trans;
        li {
            margin: 0 20px;

            a {
                font-weight: 500;
                font-size: 16px;
                display: block;
                padding: 6px 0;
                border-top: 2px solid transparent;
                border-bottom: 2px solid transparent;
                transition: $trans;

                &:hover {
                    border-bottom-color: $main
                }
            }
        }
    }
}

.headerActionMobile {
    display: none;
}

.headerActionDesktop {
    display: flex;

    .btn {
        margin-left: 20px;
    }
}
@media screen and (max-width: 1199px) {
    .headerActionMobile {
        display: flex;
    }

    .headerActionDesktop {
        display: none;
    }

    #header {
        ul.menu {
            display: none;
        }

        .headerTop {
            height: 54px;

            .logo {
                img {
                    height: 32px;
                }
            }
        }
        .headerBottom{
            display: none;
        }
    }

    #whoAreYou {
        .innerWho {
            max-width: 300px;
            margin: 20px auto 0;
        }
    }
    .header-is-green{
        .btnFull{
            color: white;
        }
    }
}

.headerSubMenu {
    background-color: $light;
    height: 0;
    overflow: hidden;
    display: flex;
    align-items: center;
    opacity: 0;
    transition: height 0.3s ease, opacity 0.3s ease;
    position: fixed; // Ajouté pour le positionnement dynamique
    z-index: 5; // En dessous du sticky mais au-dessus du contenu
    
    p {
        margin: 0 !important;
    }
}

.stickyHeader{
    position: fixed;
    top: 0px;
    left: 0;
    width: 100%;
    background-color: #fff;
    z-index: 10;
    transition: $trans;
    transform: translateY(-100%);
    .logo{
        img{
            height: 26px;
        }
        margin-right: 15px;
    }
    ul{
        display: flex;
        li{
            padding: 0 15px;
            text-align: center;
            height: 50px;
            display: flex;
            align-items: center;
            transition: $trans;
            &.active{
                background-color: $light;
            }
            span{
                font-size: 14px;
                font-weight: 500;
            }
        }
    }
    .headerActionDesktop{
        .btn{
            color: $main;
        }
    }
}

#header.show-sticky .stickyHeader {
    transform: none;
}
</style>