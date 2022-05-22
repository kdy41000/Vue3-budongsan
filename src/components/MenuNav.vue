<template>
    <nav id="primary_nav_wrap">
        <ul>
            <li v-for="menu in menuList" 
                :key="menu.id" 
                :class="{'current-menu-item': menu.clickable == true}"
                @click="clickMenuItem(menu)"
            >
                <a href="#">{{menu.name}}</a>
            </li>
        </ul>
   </nav>
</template>

<script>
export default {
    name: 'menu-nav',
    props: {
        menuArr: Array
    },
    data() {
        return {
            menuList : this.menuArr
        }
    },
    methods: {
        clickMenuItem:function(item) {
            for(let i = 0 ; i < this.menuList.length; i++) {
                this.menuList[i].clickable = false;
            }
            item.clickable = true;
        },
    }
}
</script>

<style>
#primary_nav_wrap
{
    margin-top:15px;
    padding: 50px;
}

#primary_nav_wrap ul
{
	list-style:none;
	position:relative;
	float:left;
	margin:0;
	padding:0
}

#primary_nav_wrap ul a
{
	display:block;
	color:#333;
	text-decoration:none;
	font-weight:700;
	font-size:12px;
	line-height:32px;
	padding:0 15px;
	font-family:"HelveticaNeue","Helvetica Neue",Helvetica,Arial,sans-serif
}

#primary_nav_wrap ul li
{
	position:relative;
	float:left;
	margin:0;
	padding:0
}

#primary_nav_wrap ul li.current-menu-item
{
	background:#ddd
}

#primary_nav_wrap ul li:hover
{
	background:#f6f6f6
}

#primary_nav_wrap ul ul
{
	display:none;
	position:absolute;
	top:100%;
	left:0;
	background:#fff;
	padding:0
}

#primary_nav_wrap ul ul li
{
	float:none;
	width:200px
}

#primary_nav_wrap ul ul a
{
	line-height:120%;
	padding:10px 15px
}

#primary_nav_wrap ul ul ul
{
	top:0;
	left:100%
}

#primary_nav_wrap ul li:hover > ul
{
	display:block
}
</style>