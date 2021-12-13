<script>
  import L from "leaflet";
  import { onMount } from "svelte";
  import { slide } from "svelte/transition";
  import { provinces } from "./provinces";

  let provinceName = "";
  let searchedSchool = "";
  let schoolsInProvince = [];
  let matchedSchools = [];

  //watching for any changes to the school name being searched
  //if there are changes, we filter the matching schools
  $: {
    let schoolNameUpperCase = searchedSchool.toLocaleUpperCase();
    matchedSchools = schoolsInProvince.filter((val) => {
      return val.Institution_Name.includes(schoolNameUpperCase);
    });
  }
  //watching for any changes to the selected province name
  $: {
    if (!!provinceName) {
      //get all the schools in that province
      getSchools();
    }
  }

  //get all schools in a province
  let getSchools = async () => {
    let data = await fetch(
      `https://api.mediahack.co.za/education/schools/?province=${provinceName}`
    ).then((res) => {
      return res.json();
    });
    schoolsInProvince = data;
    console.log(schoolsInProvince);
  };

  let showSchoolMap = (event) => {
    let lat = Number(event.target.firstElementChild.value);
    let long = Number(event.target.lastElementChild.value);
    let nameSchool = event.target.innerText;
    //checking if the map is already initialized or not
    var container = L.DomUtil.get("map");
    if (container != null) {
      container._leaflet_id = null;
    }

    //intializing the map
    addMap(long, lat, nameSchool);

    searchedSchool = "";
  };

  function addMap(
    long = 27.843574,
    lat = -26.394832,
    popUpText = "MADIBA PRIMARY SCHOOL"
  ) {
    var map = L.map("map").setView([lat, long], 15);

    L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
      attribution: "",
    }).addTo(map);

    L.marker([lat, long]).addTo(map).bindPopup(popUpText).openPopup();
  }

  onMount(() => {
    addMap();
  });
</script>

<div id="parent-container">
  <p class="text-4xl text-center mt-14">Search for Schools</p>
  <div
    class="flex flex-col-reverse md:grid md:grid-cols-6 gap-2 m-auto mt-10 md:w-full lg:w-5/6 l"
  >
    <div class="map-container md:col-span-4">
      <div class="ring-1" id="map" />
    </div>

    <!--SELECT PROVINCE AND SEARCH FOR A SCHOOL CONTAINER-->
    <div class="w-full md:col-span-2">
      <div class="w-full m-auto border border-blue-500">
        <p
          class="bg-blue-800 text-gray-100 text-lg font-semibold px-2 pb-6 pt-1"
        >
          South Africa School Lookup
        </p>

        <!--SELECT PROVINCE-->
        <div class="school-search m-auto my-2 p-2 bg-gray-200 rounded">
          <label class="font-semibold px-2" for="province-select">
            Province
          </label>
          <select
            bind:value={provinceName}
            id="province-select"
            name="provinces"
            class="
                    w-full
                    outline-none
                    ring-1
                    focus:ring-blue-300
                    text-gray-600
                    rounded
                    transition
                    duration-200
                    ease-in-out
                    h-8
                    px-2
                    "
          >
            <option value="">--Select Province--</option>
            {#each provinces as province}
              <option value={province.ISO}>{province.name}</option>
            {/each}
          </select>
        </div>

        <!--SEARCH FOR A SCHOOL-->
        <div class="school-search m-auto my-2 p-2 bg-gray-200 rounded">
          <p class="font-semibold px-2">Enter School Name</p>
          <div class="flex justify-evenly items-center w-full">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-6 w-6"
              viewBox="0 0 20 20"
              fill="currentColor"
            >
              <path
                fill-rule="evenodd"
                d="M5.05 4.05a7 7 0 119.9 9.9L10 18.9l-4.95-4.95a7 7 0 010-9.9zM10 11a2 2 0 100-4 2 2 0 000 4z"
                clip-rule="evenodd"
              />
            </svg>
            <input
              bind:value={searchedSchool}
              on:keypress={searchedSchool}
              class="school-search focus:outline-none h-8 rounded px-2 ring-1 ring-blue-200 transition duration-200 ease-in-out focus:ring-blue-300"
              type="text"
            />
          </div>
        </div>
      </div>
      <!-- MATCHED RESULTS LIST-->
      {#if !!searchedSchool}
        <div
          transition:slide
          class="w-full flex justify-start m-auto mb-8 border-2 border-gray-50 max-h-32 overflow-y-scroll z-10 shadow-xl rounded-b-lg"
        >
          <ul class="w-full">
            {#if matchedSchools.length > 0}
              {#each matchedSchools as matchedSchool}
                <li
                  id={matchedSchool.id}
                  on:click={showSchoolMap}
                  class="hover:text-blue-500 pl-8 pr-2 py-1 border-b-2 border-gray-100 relative cursor-pointer hover:bg-gray-200 transition duration-200 ease-in-out"
                >
                  <!--HIDDEN INPUTS THAT CONTAIN THE LAT AND LONG OF THE SCHOOL-->
                  <input type="hidden" value={matchedSchool.Lat} />
                  <input type="hidden" value={matchedSchool.Lon} />

                  {matchedSchool.Institution_Name}
                </li>
              {/each}
            {:else}
              <li
                class="hover:text-blue-500 pl-8 pr-2 py-1 border-b-2 border-gray-100 relative cursor-pointer hover:bg-gray-100"
              >
                No results
              </li>
            {/if}
          </ul>
        </div>
      {/if}
    </div>
  </div>
</div>

<style>
  #parent-container {
    width: 95%;
    margin: auto;
    font-family: "Titillium Web", sans-serif;
  }
  .school-search {
    width: 98%;
  }
  #map {
    height: 400px;
    width: auto;
  }

  @media (min-width: 768px) {
    #parent-container {
      width: 98%;
    }
  }
</style>
