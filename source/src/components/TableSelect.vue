<template>
    <form className="table-select" >

        <select v-model="sortByName" name="name" required>
            <option value="empty" disabled>Поле...</option>
            <option value="name">Название</option>
            <option value="points">Количество</option>
            <option value="distance">Расстояние</option>
        </select>

        <select v-model="sortByLaw" name="law" required> 
            <option value="empty" selected disabled>Условие...</option>
            <option value="equal">Равно</option>
            <option value="contain">Содержит</option>
            <option value="greater">Больше</option>
            <option value="less">Меньше</option>
        </select>

        <input v-model="inputText" name="argument" type="text" placeholder="Значение" required />

        <button @click.prevent="reset" className="table-select__button"> 
            {{btns[0]}}
            <!-- Сброс -->
        </button>
        <button  type="submit" @click.prevent="filter" className="table-select__button" > 
            {{btns[1]}}
            <!-- Фильтр -->
        </button>
	</form>

    <Table :pData = "paginatedData"/>
    
    <div id="pagination" >
        <paginated-button v-for="(p, index) in countOfItems" :key="index" :count="index + 1" @click="pageToStep(index)">
        </paginated-button>
    </div>

</template>

<script>
	import { ref, computed } from 'vue';
	import Table from './Table';
	import dataValue from '@/data/data.js';
    import paginatedButton from './PaginationButton';
    export default {
        components: { 
            Table,
            paginatedButton
        },

        setup () {
            let data = ref(dataValue);

            function sortByYear(arr) { //отсортируем по дате
                //быстрая сортировка sort()
                arr.sort((a, b) => a.date > b.date ? 1 : -1);
            }
            sortByYear(data.value);

            //кнопочки
            const pageNumber = ref(0); // по умолчанию 0;
            let size = 5;  
            // let notesOnPage = 2;
            // let countOfItems = Math.ceil(data.value.length / size); //округляем в большую сторону
            const countOfItems = computed(() => {
                return Math.ceil(data.value.length / size); //округляем в большую сторону
            })

            //отфильтрованные данные, которые будут отображаться
			const paginatedData = computed (() => {
                if (data.value.length < size) {
                    return data.value;
                } else {
                    const start = pageNumber.value * size,
					    end = start + size;
 				return data.value.slice(start, end);
                }
			});
            function pageToStep(step) {
				pageNumber.value = step ;
			};

            //ранжирование
            let btns = ['Сброс', 'Фильтр' ];
            const sortByName = ref('empty'); //присвоили переменной sortByName значение 'nameEmpty', ref-делаем реактивной (наблюдаем)
            const sortByLaw = ref('empty'); 
            const inputText = ref ('');

            // const filter = computed (()=> {
            function filter() {
                let text ="";
                if ((inputText.value !== undefined || inputText.value !== null) && inputText.value === ""){ 
                    text = inputText.placeholder;
                }
                else {
                    text = inputText.value; 
                }
                switch (sortByLaw.value) {
                    case 'equal': { //==
                        data.value = data.value.filter(item => item[sortByName.value] == text);
                        break;
                    }   
                    case 'contain': { //содержит
                        data.value = data.value.filter(item => item[sortByName.value].toString().includes(text));
                        break; 
                    }   
                    case 'less': { //<
                        data.value = data.value.filter(item => item[sortByName.value] < text);
                        break;
                    }   
                    case 'greater': {//>
                        data.value = data.value.filter(item => item[sortByName.value] > text); 
                        break;
                    }   
                    default:
                        break;
                }
             
            };

            function reset() {
                data.value = dataValue;
                inputText.value = "";
            };

			return {
                data, 
                // sorteredItems, 
                sortByName,
                sortByLaw,
                inputText,
                filter, 
                reset,
                btns, pageToStep, countOfItems, paginatedData,
            }
		}
    }
</script>


